---
title: Vues dans ASP.NET Core MVC
author: ardalis
description: Découvrez comment les vues gèrent la présentation des données et les interactions utilisateur dans les applications ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 12/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/overview
ms.openlocfilehash: b9af2068aec4326585eb2a8994399a16461db3be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2018
---
# <a name="views-in-aspnet-core-mvc"></a>Vues dans ASP.NET Core MVC

Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)

Ce document explique l’utilisation des vues dans les applications ASP.NET Core MVC. Pour plus d’informations sur les pages Razor, consultez [Présentation des pages Razor](xref:mvc/razor-pages/index).

Selon le schéma MVC (**M**odèle-**V**ue-**C**ontrôleur, la *vue* gère la présentation des données et les interactions utilisateur dans l’application. Une vue est un modèle HTML dans lequel est incorporé un [balisage Razor](xref:mvc/views/razor). Le balisage Razor est du code qui interagit avec le balisage HTML pour générer une page web qui est envoyée au client.

Dans ASP.NET Core MVC, les vues sont des fichiers *.cshtml* qui utilisent le [langage de programmation C#](/dotnet/csharp/) dans le balisage Razor. En règle générale, les fichiers de vue sont regroupés dans des dossiers nommés correspondant aux différents [contrôleurs](xref:mvc/controllers/actions) de l’application. Ces dossiers sont eux-mêmes regroupés sous le dossier *Vues* situé à la racine de l’application :

![Dossier Vues développé dans l’Explorateur de solutions de Visual Studio, avec le dossier Accueil ouvert contenant les fichiers About.cshtml, Contact.cshtml et Index.cshtml](overview/_static/views_solution_explorer.png)

Le contrôleur *Home* est représenté par un dossier *Home* à l’intérieur du dossier *vues*. Le dossier *Home* contient les vues pour les pages web *About*, *Contact*, et *Index* (page d’accueil). Quand un utilisateur demande une de ces trois pages Web, les actions de contrôleur dans le contrôleur *Home* sont utilisées afin de déterminer laquelle des trois vues est utilisée pour générer et retourner une page Web à l’utilisateur.

Utilisez [dispositions](xref:mvc/views/layout) pour fournir des sections cohérentes de page Web et de réduire la redondance du code. Les dispositions contiennent souvent l’en-tête, des éléments de menu et de navigation, et le pied de page. L’en-tête et le pied de page contiennent généralement des balises standard pour de nombreux éléments de métadonnées et des liens vers des ressources de script et de style. Les dispositions vous aident à éviter ce balisage réutilisable dans vos vues.

Les [vues partielles](xref:mvc/views/partial) réduisent la répétition de code grâce à la gestion des parties réutilisables dans les vues. Par exemple, une vue partielle est utile dans le cas d’une biographie d’auteur publiée sur un site web de blog qui doit s’afficher dans plusieurs vues. Une biographie d’auteur présente un contenu de vue standard et ne nécessite pas d’exécution de code particulier pour générer le contenu à afficher sur la page web. Le contenu de la biographie d’auteur est fourni à la vue uniquement par la liaison de données. C’est pourquoi l’utilisation d’une vue partielle pour ce type de contenu est la solution la plus appropriée.

Les [composants de vue](xref:mvc/views/view-components) sont similaires aux vues partielles dans le sens où ils vous permettent aussi de réduire la répétition de code. La différence est qu’ils sont plutôt conçus pour du contenu de vue qui nécessite l’exécution de code sur le serveur pour afficher la page web. Les composants de vue sont utiles quand le contenu affiché doit interagir avec une base de données, comme c’est le cas pour un panier d’achat sur un site web. Les composants de vue ne dépendent pas d’une liaison de données pour pouvoir générer la sortie d’une page web.

## <a name="benefits-of-using-views"></a>Avantages de l’utilisation des vues

Les vues permettent d’établir une [ **S**eparation **o**f **C**oncerns (SoC) conception](http://deviq.com/separation-of-concerns/) au sein d’une application MVC en séparant le balisage de l'interface utilisateur des autres parties de l’application. Suivre les principes de la conception SoC rend votre application modulaire, ce qui offre plusieurs avantages :

* L’application est plus facile à gérer, car elle est mieux organisée. Les vues sont généralement regroupées en fonction de l’application. Cela rend plus facile à trouver les vues associées lorsque vous travaillez sur une fonctionnalité.
* Les parties de l’application sont faiblement couplées. Vous pouvez créer et mettre à jour des vues de l’application séparément dans les composants de logique métier et d’accès aux données. Vous pouvez modifier les vues de l’application sans nécessairement devoir mettre à jour des autres parties de l’application.
* Il est plus facile de tester les parties de l’interface utilisateur de l’application, car les vues constituent des unités individuelles séparées.
* En raison d’une meilleure organisation, le risque est moins grand de répéter accidentellement des sections de l’interface utilisateur.

## <a name="creating-a-view"></a>Création d’une vue

Les vues qui sont spécifiques à un contrôleur sont créées dans le dossier *Views/[nom du contrôleur]*. Les vues qui sont partagées entre les contrôleurs sont placées dans le dossier *Views/Shared*. Pour créer une vue, ajoutez un nouveau fichier et donnez-lui le même nom que son action de contrôleur associée avec l’extension de fichier *.cshtml*. Pour créer une vue qui correspond à l'action *About* dans le contrôleur *Home*, créez un fichier *About.cshtml* dans le dossier *Views/Home* :

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

La syntaxe *Razor* commence par le symbole `@`. Exécutez des instructions c# en plaçant du code c# dans des [blocs de code Razor](xref:mvc/views/razor#razor-code-blocks) délimités par des accolades (`{ ... }`). Par exemple, consultez l’affectation de « About » pour `ViewData["Title"]` ci-dessus. Vous pouvez afficher des valeurs dans le code HTML en référençant simplement la valeur avec le symbole `@`. Consultez le contenu des éléments `<h2>` et `<h3>` ci-dessus.

Le contenu de la vue ci-dessus n'est qu’une partie de la page Web entière qui est restituée à l’utilisateur. Le reste de la mise en page et d’autres aspects courants de la vue sont spécifiés dans d’autres fichiers de vue. Pour plus d’informations, consultez la [rubrique de présentation](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Comment les contrôleurs spécifient les vues

Les vues sont généralement retournées par des actions sous forme d’objet [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), qui est un type d’objet [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). Votre méthode d’action peut créer et retourner un `ViewResult` directement, mais cela n’est pas le plus fréquent. Étant donné que la plupart des contrôleurs héritent de [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), utilisez simplement la méthode d’assistance `View` pour retourner le `ViewResult` :

*HomeController.cs*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Lorsque cette action est retournée, la vue *About.cshtml* indiquée dans la dernière section est restituée en tant que page Web :

![Page About affichée dans le navigateur Edge](overview/_static/about-page.png)

La méthode helper `View` a plusieurs surcharges. Vous pouvez éventuellement spécifier :

* Une vue explicite à retourner :

  ```csharp
  return View("Orders");
  ```
* Un [modèle](xref:mvc/models/model-binding) à passer à la vue :

  ```csharp
  return View(Orders);
  ```
* Une vue et un modèle :

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Détection de la vue

Lorsqu’une action retourne une vue, un processus appelé *détection de la vue* a lieu. Ce processus détermine quel fichier est utilisé en fonction du nom de la vue. 

Le comportement par défaut de la méthode `View` (`return View();`) est de retourner une vue du même nom que la méthode d’action à partir de laquelle elle est appelée. Par exemple, le nom de la méthode `ActionResult` *About* du contrôleur est utilisé pour rechercher un fichier de vue nommé *About.cshtml*. Le Runtime commence par rechercher la vue dans le dossier *Vues/[nom_contrôleur]*. S’il ne trouve pas de vue correspondante, il recherche ensuite la vue dans le dossier *Partagé*.

Peu importe si vous retournez implicitement `ViewResult` avec `return View();` ou si vous passez explicitement le nom de la vue à la méthode `View` avec `return View("<ViewName>");`. Dans les deux cas, la détection de la vue recherche un fichier de vue correspondant dans cet ordre :

   1. *Vues /\[ControllerName] /\[ViewName] .cshtml*
   1. *Vues/Partagé/\[nom_vue].cshtml*

Vous pouvez spécifier le chemin du fichier de vue au lieu du nom de la vue. Si vous utilisez un chemin absolu à partir de la racine de l’application (commençant éventuellement par « / » ou « ~/ »), vous devez indiquer l’extension *.cshtml* :

```csharp
return View("Views/Home/About.cshtml");
```

Vous pouvez également utiliser un chemin relatif pour spécifier des vues situées dans des répertoires différents. Dans ce cas, n’indiquez pas l’extension *.cshtml*. Dans `HomeController`, vous pouvez retourner la vue *Index* du dossier de vues *Gérer* avec un chemin relatif :

```csharp
return View("../Manage/Index");
```

De même, vous pouvez indiquer le dossier spécifique du contrôleur actif avec le préfixe « ./ » :

```csharp
return View("./About");
```

Les [vues partielles](xref:mvc/views/partial) et les [composants de vue](xref:mvc/views/view-components) utilisent des mécanismes de détection presque identiques.

Vous pouvez personnaliser la convention par défaut de recherche des vues dans l’application à l’aide d’un [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander) personnalisé.

La détection des vues recherche les fichiers de vue d’après le nom de fichier. Si le système de fichiers sous-jacent respecte la casse, les noms de vue respectent probablement la casse eux aussi. Pour garantir la compatibilité entre les systèmes d’exploitation, utilisez la même casse pour les noms de contrôleur et d’action et pour les noms de leurs dossiers et fichiers de vues associés. Si vous utilisez un système de fichiers qui respecte la casse et que vous voyez un message d’erreur indiquant qu’un fichier de vue est introuvable, vérifiez que le nom du fichier de vue demandé et le nom du fichier de vue réel ont une casse identique.

Suivez les bonnes pratiques en matière d’organisation de la structure des fichiers de vue. Votre structure doit refléter au mieux les relations entre les contrôleurs, les actions et les vues pour être plus claire et facile à gérer.

## <a name="passing-data-to-views"></a>Passage de données aux vues

Vous pouvez passer des données pour les vues à l’aide de plusieurs approches. L’approche la plus fiable consiste à spécifier un [modèle](xref:mvc/models/model-binding) type dans la vue. Ce modèle est communément appelé un *viewmodel*. Vous passez une instance du type viewmodel à la vue à partir de l’action.

L’utilisation d’un ViewModel pour passer des données à une vue vous permet d’effectuer un contrôle de type *fort* dans la vue. Le terme *typage fort* (ou *fortement typé*) signifie que chaque variable et constante a un type défini explicitement (par exemple, `string`, `int` ou `DateTime`). La validité des types utilisés dans une vue est contrôlée au moment de la compilation.

[Visual Studio](https://www.visualstudio.com/vs/) et [Visual Studio Code](https://code.visualstudio.com/) répertorient les membres de classe fortement typés à l’aide d’une fonctionnalité appelée [IntelliSense](/visualstudio/ide/using-intellisense). Quand vous voulez afficher les propriétés d’un ViewModel, tapez le nom de variable pour le ViewModel suivi d’un point (`.`). Cela vous permet d’écrire du code plus rapidement et avec moins d’erreurs.

Spécifiez un modèle à l’aide de la directive `@model`. Utilisez le modèle avec `@Model` :

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Pour fournir le modèle à la vue, le contrôleur le passe en tant que paramètre :

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

Il n’existe aucune restriction sur les types de modèles que vous pouvez fournir à une vue. Nous vous recommandons d’utiliser des modèles de vue **P****O****C****O**avec peu ou pas de comportements (méthodes) définis. En règle générale, les classes viewmodel sont soit stockées dans le dossier *Models* ou dans un dossier *ViewModels* distinct à la racine de l’application. Le viewmodel *Adress* utilisé dans l’exemple ci-dessus est un viewmodel POCO stocké dans un fichier nommé *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> Rien ne vous empêche d’utiliser les mêmes classes pour vos types de ViewModel et vos types de modèle métier. Toutefois, l’utilisation de modèles distincts vous permet de changer les vues indépendamment de la logique métier et des composants d’accès aux données de votre application. La séparation des modèles et des ViewModel est également un gage de sécurité si vous avez des modèles qui utilisent la [liaison de données](xref:mvc/models/model-binding) et la [validation](xref:mvc/models/validation) pour les données envoyées à l’application par l’utilisateur.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Données faiblement typées (ViewData et ViewBag)

Remarque : `ViewBag` n’est pas disponible dans les pages Razor.

En plus des vues fortement typées, les vues ont accès à une collection de données *faiblement typées* (ou *librement typées*). Contrairement aux types forts, les *types faibles* (ou *types libres*) ne nécessitent pas de déclarer explicitement le type de données utilisé. Vous pouvez utiliser la collection de données faiblement typées pour passer de petites quantités de données entre les contrôleurs et les vues.

| Passer des données entre...                        | Exemple                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Un contrôleur et une vue                             | Remplissage d’une liste déroulante avec des données.                                          |
| Une vue et une [disposition](xref:mvc/views/layout)   | Définition du contenu de l’élément **\<title>** dans la disposition à partir d’un fichier de vue.  |
| Une [vue partielle](xref:mvc/views/partial) et une vue | Widget qui affiche des données en fonction de la page web demandée par l’utilisateur.      |

Cette collection peut être référencée par les propriétés `ViewData` ou `ViewBag` sur les contrôleurs et les vues. La propriété `ViewData` est un dictionnaire d’objets faiblement typés. La propriété `ViewBag` est un wrapper autour de `ViewData` qui fournit des propriétés dynamiques pour la collection `ViewData` sous-jacente.

`ViewData` et `ViewBag` sont résolues dynamiquement au moment de l’exécution. Dans la mesure où elles n’effectuent pas de contrôle de type à la compilation, ces deux propriétés sont généralement davantage sujettes aux erreurs qu’un ViewModel. Pour cette raison, certains développeurs préfèrent ne jamais utiliser les propriétés `ViewData` et `ViewBag`, ou les utiliser le moins possible.


<a name="VD"></a>

**ViewData**

`ViewData` est un objet [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) accessible à l’aide de clés `string`. Vous pouvez stocker et utiliser des données de type chaîne directement, sans avoir à les caster, mais vous devez effectuer un cast des autres valeurs de l’objet `ViewData` vers des types spécifiques lors de leur extraction. Vous pouvez utiliser `ViewData` pour passer des données des contrôleurs aux vues et au sein même des vues, y compris les [vues partielles](xref:mvc/views/partial) et les [dispositions](xref:mvc/views/layout).

L’exemple suivant utilise un objet `ViewData` dans une action pour définir les valeurs d’un message d’accueil et d’une adresse :

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Utilisation des données dans une vue :

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

Remarque : `ViewBag` n’est pas disponible dans les pages Razor.

`ViewBag` est un objet [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) qui fournit un accès dynamique aux objets stockés dans `ViewData`. `ViewBag` est parfois plus pratique à utiliser, car il ne nécessite pas de cast. L’exemple suivant montre comment utiliser `ViewBag` pour obtenir le même résultat qu’avec l’objet `ViewData` ci-dessus :

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**Utilisation simultanée de ViewData et ViewBag**

Remarque : `ViewBag` n’est pas disponible dans les pages Razor.

Comme `ViewData` et `ViewBag` font référence à la même collection `ViewData` sous-jacente, vous pouvez utiliser `ViewData` et `ViewBag` simultanément, en les combinant et en les associant pour lire et écrire des valeurs.

Définissez le titre avec `ViewBag` et la description avec `ViewData` au début d’une vue *About.cshtml* :

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Lisez les propriétés, mais inversez l’utilisation de `ViewData` et `ViewBag`. Dans le fichier *_Layout.cshtml*, obtenez le titre et la description avec `ViewData` et `ViewBag`, respectivement :

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Souvenez-vous que les chaînes ne nécessitent pas de cast pour `ViewData`. Vous pouvez utiliser `@ViewData["Title"]` sans cast.

L’utilisation simultanée de `ViewData` et `ViewBag` est possible, de la même manière que la combinaison et l’association des propriétés de lecture et d’écriture. Le balisage suivant est affiché :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Récapitulatif des différences entre ViewData et ViewBag**

 `ViewBag` n’est pas disponible dans les pages Razor.

* `ViewData`
  * Dérivé de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), cet objet fournit des propriétés de dictionnaire potentiellement utiles, telles que `ContainsKey`, `Add`, `Remove` et `Clear`.
  * Les clés contenues dans le dictionnaire sont des chaînes ; les espaces blancs sont donc autorisés. Exemple : `ViewData["Some Key With Whitespace"]`
  * Les autres types que `string` doivent être castés dans la vue pour utiliser `ViewData`.
* `ViewBag`
  * Dérivé de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), cet objet permet de créer des propriétés dynamiques avec la notation par points (`@ViewBag.SomeKey = <value or object>`). Aucun cast n’est nécessaire. La syntaxe de `ViewBag` facilite son ajout aux contrôleurs et aux vues.
  * Simplifie la vérification des valeurs Null. Exemple : `@ViewBag.Person?.Name`

**Quand utiliser ViewData ou ViewBag ?**

`ViewData` et `ViewBag` constituent deux approches appropriées pour passer de petites quantités de données entre les contrôleurs et les vues. Choisissez celle qui vous convient le mieux. Vous pouvez combiner et associer les objets `ViewData` et `ViewBag`. Toutefois, il est recommandé d’utiliser une seule approche pour faciliter la lecture et la gestion du code. Les deux approches sont résolues dynamiquement au moment de l’exécution et sont donc susceptibles d’engendrer des erreurs d’exécution. C’est la raison pour laquelle certains développeurs préfèrent ne pas les utiliser.

### <a name="dynamic-views"></a>Vues dynamiques

Les vues qui ne déclarent pas de modèle de type à l’aide de `@model` mais qui reçoivent une instance de modèle (par exemple, `return View(Address);`) peuvent référencer dynamiquement les propriétés de l’instance :

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Cette fonctionnalité offre beaucoup de flexibilité, mais elle ne fournit pas de protection de la compilation ou la fonction IntelliSense. Si la propriété n’existe pas, la génération de la page web échoue au moment de l’exécution.

## <a name="more-view-features"></a>Autres fonctionnalités de vue

Les [Tag Helpers](xref:mvc/views/tag-helpers/intro) permettent d’ajouter facilement un comportement côté serveur dans des balises HTML existantes. Leur utilisation vous évite d’avoir à écrire du code personnalisé ou des méthodes d’assistance dans les vues. Les Tag Helpers sont appliqués comme attributs aux éléments HTML et sont ignorés par les éditeurs qui ne peuvent pas les traiter. Vous pouvez ainsi modifier et afficher le balisage des vues dans divers outils.

Générer un balisage HTML personnalisé peut être obtenu avec de nombreux helpers HTML intégrés. Une logique plus complexe de l’interface utilisateur peut être gérée par des [composants de vue](xref:mvc/views/view-components). Les composants de vue fournissent la même séparation de responsabilité (SoC) que les contrôleurs et les vues offrent. Il peuvent éliminer le besoin d'utiliser des actions ou des vues qui traitent les données utilisées par les éléments d’interface couramment utilisés.

Comme de nombreux autres aspects d’ASP.NET Core, les vues prennent en charge [l’injection de dépendances](xref:fundamentals/dependency-injection), ce qui permet aux services d’être [injectés dans les vues](xref:mvc/views/dependency-injection).
