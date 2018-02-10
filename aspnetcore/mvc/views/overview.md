---
title: "Vues de base d’ASP.NET MVC"
author: ardalis
description: "Découvrez comment gérer les vues de présentation des données de l’application et l’interaction utilisateur dans ASP.NET MVC de base."
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: dc36c76dbd7d82a926e39d8a8ab3a2a53b65d954
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="views-in-aspnet-core-mvc"></a>Les vues dans ASP.NET Core MVC

Par [Steve Smith](https://ardalis.com/) et [Luke Latham](https://github.com/guardrex)

Ce document explique les vues utilisées dans les applications ASP.NET Core MVC. Pour plus d’informations sur les Pages Razor, consultez [Introduction aux Pages Razor](xref:mvc/razor-pages/index).

Dans le **M**odèle -**V**UE -**C**ontroller (MVC), modèle, le *vue* gère l’interaction utilisateur et de présentation des données de l’application. Une vue est un modèle HTML incorporé [balisage de Razor](xref:mvc/views/razor). Balisage de Razor est un code qui interagit avec le balisage HTML pour générer une page Web qui est envoyée au client.

Dans ASP.NET MVC de base, les vues sont *.cshtml* fichiers qui utilisent le [langage de programmation c#](/dotnet/csharp/) dans le balisage de Razor. En règle générale, afficher les fichiers sont regroupés dans des dossiers nommés pour chacune de l’application [contrôleurs](xref:mvc/controllers/actions). Les dossiers sont stockées dans un *vues* dossier à la racine de l’application :

![Dossier de vues dans l’Explorateur de solutions de Visual Studio est ouvert avec le dossier de base ouvert pour afficher les fichiers About.cshtml, Contact.cshtml et Index.cshtml](overview/_static/views_solution_explorer.png)

Le contrôleur *accueil* est représenté par un dossier *accueil* à l’intérieur du dossier *vues*. Le dossier *accueil* contient les vues pour les pages web *A propos de*, *Contact*, et *Index* (page d’accueil). Quand un utilisateur demande une de ces trois pages Web, les actions de contrôleur dans le contrôleur *accueil* sont utilisées afin de déterminer laquelle des trois vues est utilisée pour générer et retourner une page Web à l’utilisateur.

Utilisez [dispositions](xref:mvc/views/layout) pour fournir des sections cohérentes de page Web et de réduire la redondance du code. Les dispositions contiennent souvent l’en-tête, des éléments de menu et de navigation, et le pied de page. L’en-tête et le pied de page contiennent généralement des balises standard pour de nombreux éléments de métadonnées et des liens vers des ressources de script et de style. Les dispositions vous aident à éviter ce balisage réutilisable dans vos vues.

[Les vues partielles](xref:mvc/views/partial) réduisent la duplication de code grâce à la gestion de parties réutilisables de vues. Par exemple, une vue partielle est utile pour la biographie de l’auteur sur un site Web de blog qui apparaît dans plusieurs vues. La biographie de l’auteur est d’ordinaire afficher le contenu et ne nécessite pas le code à exécuter afin de produire le contenu de la page Web. Créer du contenu biographie est disponible à l’affichage par la liaison de modèle uniquement, par conséquent, à l’aide d’une vue partielle pour ce type de contenu est idéale.

[Composants de vue](xref:mvc/views/view-components) ont le même principe que les vues partielles dans la mesure où ils vous permettent de réduire le code répétitif, mais ils sont appropriés pour l’affichage de contenu qui nécessite l'exécution de code côté serveur pour restituer la page Web. Les composants de vue sont utiles lorsque le contenu rendu requiert une interaction de base de données, comme pour un site Web du panier d’achat. Les composants de vue ne sont pas limitées à la liaison de modèle afin de produire le résultat de la page Web.

## <a name="benefits-of-using-views"></a>Avantages de l’utilisation de vues

Les vues permettent d’établir une [ **S**eparation **o**f **C**oncerns (SoC) conception](http://deviq.com/separation-of-concerns/) au sein d’une application MVC en séparant le balisage de l'interface utilisateur à partir d'autres parties de l’application. Suivre le principe de SoC rend votre application modulaire, ce qui offre plusieurs avantages :

* L’application est plus facile à gérer, car elle est mieux organisée. Les vues sont généralement regroupées en fonction de l’application. Cela rend plus facile à trouver les vues associées lorsque vous travaillez sur une fonctionnalité.
* Les parties de l’application sont faiblement couplées. Vous pouvez générer et mettre à jour des vues de l’application séparément dans les composants accès logique et les données d’entreprise. Vous pouvez modifier les vues de l’application sans nécessairement devoir mettre à jour des autres parties de l’application.
* Il est plus facile de tester des parties de l’interface utilisateur de l’application, car les vues sont des unités distinctes.
* En raison d’une meilleure organisation, il est moins probable que vous alliez accidentellement dupliquer des sections de l’interface utilisateur.

## <a name="creating-a-view"></a>Création d’une vue

Les vues qui sont spécifiques à un contrôleur sont créées dans le dossier *vues / [nom du contrôleur]*. Les vues qui sont partagées entre les contrôleurs sont placées dans le dossier *Views/Shared*.  Pour créer une vue, ajoutez un nouveau fichier et lui donner le même nom que son action de contrôleur associé avec le fichier *.cshtml*. Pour créer une vue qui correspond à l'action *a propos de* dans le contrôleur *accueil*, créez un fichier *About.cshtml* dans le dossier *Views/Home* :

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

La syntaxe *Razor* commence par le symbole `@`. Exécutez des instructions c# en plaçant du code c# dans des [blocs de code Razor](xref:mvc/views/razor#razor-code-blocks) délimités par des accolades (`{ ... }`). Par exemple, consultez l’affectation de « About » pour `ViewData["Title"]` ci-dessus. Vous pouvez afficher des valeurs dans le code HTML en référençant simplement la valeur avec le symbole `@`. Consultez le contenu des éléments `<h2>` et `<h3>` ci-dessus.

Le contenu de la vue ci-dessus n'est qu’une partie de la page Web entière qui est restituée à l’utilisateur. Le reste de la mise en page et d’autres aspects courants de la vue sont spécifiés dans d’autres fichiers de vue. Pour plus d’informations, consultez la [rubrique de présentation](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Comment les contrôleurs spécifient les vues

Les vues sont généralement retournées à partir d’actions en tant que [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), qui est un type de [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). Votre méthode d’action peut créer et renvoyer un `ViewResult` directement, mais cela n’est pas généralement fait. Étant donné que la plupart des contrôleurs héritent de [contrôleur](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), vous pouvez utiliser simplement la méthode `View` pour retourner le `ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Lorsque cette action est retournée, la vue *About.cshtml* indiquée dans la dernière section est restituée en tant que page Web :

![Sur la page rendue dans le navigateur Edge](overview/_static/about-page.png)

La méthode `View` a plusieurs surcharges. Vous pouvez éventuellement spécifier :

* Un affichage explicite pour renvoyer :

  ```csharp
  return View("Orders");
  ```
* Un [modèle](xref:mvc/models/model-binding) à passer à la la vue :

  ```csharp
  return View(Orders);
  ```
* Une vue et un modèle :

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Détection de la vue

Lorsqu’une action retourne une vue, un processus appelé *détection de la vue* a lieu. Ce processus détermine quel fichier est utilisé en fonction du nom de la vue. 

Le comportement par défaut de la `View` (méthode) (`return View();`) doit retourner une vue avec le même nom que la méthode d’action à partir de laquelle elle est appelée. Par exemple, le *sur* `ActionResult` nom de la méthode du contrôleur est utilisé pour rechercher un fichier de vue nommé *About.cshtml*. Tout d’abord, le runtime recherche le *vues / [nom du contrôleur]* dossier pour l’affichage. S’il ne trouve pas une vue correspondante, il recherche le *Shared* dossier pour l’affichage.

Peu importe si vous retournez implicitement le `ViewResult` avec `return View();` ou explicitement passer le nom d’affichage pour le `View` méthode avec `return View("<ViewName>");`. Dans les deux cas, afficher les recherches de découverte pour un fichier de vue correspondant dans cet ordre :

   1. *Views/\[ControllerName]\[ViewName].cshtml*
   1. *Views/Shared/\[ViewName].cshtml*

Un chemin d’accès du fichier de vue peut être fourni au lieu d’un nom de la vue. Si vous utilisez un chemin d’accès absolu, commençant à la racine de l’application (éventuellement en commençant par « / » ou « ~ / »), le *.cshtml* extension doit être spécifiée :

```csharp
return View("Views/Home/About.cshtml");
```

Vous pouvez également utiliser un chemin d’accès relatif pour spécifier les vues dans des répertoires différents sans le *.cshtml* extension. À l’intérieur de la `HomeController`, vous pouvez retourner le *Index* afficher de votre *gérer* vues avec un chemin d’accès relatif :

```csharp
return View("../Manage/Index");
```

De même, vous pouvez indiquer le répertoire spécifique du contrôleur actif avec le «. / » préfixe :

```csharp
return View("./About");
```

[Les vues partielles](xref:mvc/views/partial) et [affichage des composants](xref:mvc/views/view-components) utilisent des mécanismes de découverte similaires (mais non identiques).

Vous pouvez personnaliser la convention par défaut pour comment les vues sont situés dans l’application à l’aide d’un [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Découverte de la vue s’appuie sur la recherche d’afficher les fichiers par nom de fichier. Si le système de fichiers sous-jacent respecte la casse, les noms des vues sont probablement respecte la casse. Pour la compatibilité entre les systèmes d’exploitation, respecter la casse entre le contrôleur et les noms d’actions et associé à afficher les dossiers et les noms de fichiers. Si vous rencontrez une erreur Impossible de trouver un fichier de vue lorsque vous travaillez avec un système de fichiers respectant la casse, vérifiez que la casse identique dans le fichier de la vue demandée et le nom de fichier réel de l’affichage.

Suivre la meilleure pratique d’organiser la structure de fichiers pour les vues afin de refléter les relations entre les contrôleurs, les actions et les vues à la facilité de maintenance et de clarté.

## <a name="passing-data-to-views"></a>Passage de données à des vues

Vous pouvez passer des données pour les vues à l’aide de plusieurs approches. L’approche la plus fiable consiste à spécifier un [modèle](xref:mvc/models/model-binding) type dans la vue. Ce modèle est communément appelé un *viewmodel*. Vous passez une instance du type viewmodel à la vue à partir de l’action.

À l’aide d’un viewmodel pour passer des données à une vue permet de tirer parti de la vue de *fort* la vérification du type. *Un typage fort* (ou *fortement typé*) signifie que chaque variable et constante a un type défini explicitement (par exemple, `string`, `int`, ou `DateTime`). La validité des types utilisés dans une vue est vérifiée au moment de la compilation.

[Visual Studio](https://www.visualstudio.com/vs/) et [Visual Studio Code](https://code.visualstudio.com/) répertorient les membres de classe fortement typée à l’aide d’une fonctionnalité appelée [IntelliSense](/visualstudio/ide/using-intellisense). Lorsque vous souhaitez afficher les propriétés d’un viewmodel, tapez le nom de variable pour le viewmodel suivi d’un point (`.`). Cela vous permet d’écrire du code plus rapidement avec moins d’erreurs.

Spécifier un modèle à l’aide de la `@model` la directive. Utiliser le modèle avec `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Pour fournir le modèle à la vue, le contrôleur transmet en tant que paramètre :

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

Il n’existe aucune restriction sur les types de modèles que vous pouvez fournir à une vue. Nous vous recommandons d’utiliser **P**brut **O**%ld **C**LR **O**ViewModel objet (POCO) avec peu ou pas de comportement (méthodes) défini. En règle générale, les classes viewmodel sont soit stockées dans le dossier  *modèles* ou distinct du dossier *ViewModel* à la racine de l’application. Le viewmodel *adresse*  utilisé dans l’exemple ci-dessus est un viewmodel POCO stocké dans un fichier nommé *Address.cs*:

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
> Rien ne vous empêche d’utiliser les mêmes classes pour vos types viewmodel et vos types de modèle d’entreprise. Toutefois, à l’aide des modèles distincts permet à vos vues varier indépendamment à partir de la logique métier et les données des accès des parties de votre application. Séparation des modèles et ViewModel offre également des avantages de sécurité lorsque les modèles utilisent [liaison de modèle](xref:mvc/models/model-binding) et [validation](xref:mvc/models/validation) pour les données envoyées à l’application par l’utilisateur.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Données faiblement typée (ViewData et ViewBag)

Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.

Outre les vues fortement typées, vues ont accès à un *faiblement typée* (également appelé *faiblement typé*) collecte des données. Contrairement aux types forts, *types faibles* (ou *de perdre des types*) signifie que vous ne déclariez pas explicitement le type de données que vous utilisez. Vous pouvez utiliser la collection de données faiblement typé pour le passage de petites quantités de données vers et depuis les contrôleurs et les vues.

| Passer des données entre un...                        | Exemple                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Contrôleur et une vue                             | Remplissage d’une liste déroulante avec des données.                                          |
| Affichage et un [mode](xref:mvc/views/layout)   | Définition de la  **\<titre >** contenu de l’élément dans la vue mise en page à partir d’un fichier de vue.  |
| [Vue partielle](xref:mvc/views/partial) et une vue | Un widget qui affiche des données en fonction de la page Web demandée par l’utilisateur.      |

Cette collection peut être référencée par le biais du `ViewData` ou `ViewBag` propriétés sur les contrôleurs et vues. Le `ViewData` propriété est un dictionnaire d’objets de faiblement typée. Le `ViewBag` propriété est un wrapper autour de `ViewData` qui fournit des propriétés dynamiques pour sous-jacent `ViewData` collection.

`ViewData`et `ViewBag` sont résolues dynamiquement lors de l’exécution. Dans la mesure où ils n’offrent pas de vérification de type lors de la compilation, les deux sont généralement plus sujettes aux erreurs que l’utilisation d’un modèle de vues. Pour cette raison, certains développeurs préfèrent minimale ou jamais utiliser `ViewData` et `ViewBag`.


<a name="VD"></a>

**ViewData**

`ViewData`est un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) objet accédé via `string` clés. Données de chaîne peuvent être stockées et utilisées directement, sans la nécessité d’un cast, mais vous devez effectuer un cast des autres `ViewData` valeurs à des types spécifiques de l’objet lorsque vous extrayez les. Vous pouvez utiliser `ViewData` pour passer des données à partir des contrôleurs vers des affichages et dans les affichages, y compris [vues partielles](xref:mvc/views/partial) et [dispositions](xref:mvc/views/layout).

Voici un exemple qui définit les valeurs pour un message d’accueil et une adresse à l’aide `ViewData` dans une action :

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

Utiliser les données dans une vue :

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

Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.

`ViewBag`est un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) objet qui fournit un accès dynamique pour les objets stockés dans `ViewData`. `ViewBag`peut être plus pratique d’utiliser, car il ne nécessite un transtypage. L’exemple suivant montre comment utiliser `ViewBag` avec le même résultat qu’à l’aide de `ViewData` ci-dessus :

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

**À l’aide de ViewData et ViewBag simultanément**

Remarque : `ViewBag` n’est pas disponible dans les Pages Razor.

Étant donné que `ViewData` et `ViewBag` font référence au même sous-jacent `ViewData` collection, vous pouvez utiliser les deux `ViewData` et `ViewBag` et combiner entre eux lors de la lecture et l’écriture de valeurs.

Définir le titre à l’aide `ViewBag` et la description à l’aide `ViewData` en haut d’un *About.cshtml* vue :

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Lire les propriétés, mais l’inverse de l’utilisation de `ViewData` et `ViewBag`. Dans le *_Layout.cshtml* de fichiers, d’obtenir le titre à l’aide de `ViewData` et obtenez la description en utilisant `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Souvenez-vous que les chaînes ne nécessitent un cast pour `ViewData`. Vous pouvez utiliser `@ViewData["Title"]` sans cast.

L’utilisation des deux `ViewData` et `ViewBag` sur les travaux de même temps, en tant que fait mélangeant et en lire et écrire les propriétés. Le balisage suivant est rendu :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Résumé des différences entre ViewData et ViewBag**

 `ViewBag`n’est pas disponible dans les Pages Razor.

* `ViewData`
  * Dérive de [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), de sorte qu’elle possède des propriétés du dictionnaire qui peuvent être utiles, telles que `ContainsKey`, `Add`, `Remove`, et `Clear`.
  * Clés du dictionnaire sont des chaînes, un espace blanc est autorisé. Exemple : `ViewData["Some Key With Whitespace"]`
  * Tout type autre qu’un `string` doit être effectué dans la vue à utiliser `ViewData`.
* `ViewBag`
  * Dérive de [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), donc il permet la création de propriétés dynamiques à l’aide de la notation par points (`@ViewBag.SomeKey = <value or object>`), et aucune conversion n’est requise. La syntaxe de `ViewBag` rend plus rapide de les ajouter à des contrôleurs et vues.
  * La plus simple vérifier les valeurs null. Exemple : `@ViewBag.Person?.Name`

**Quand utiliser ViewData ou ViewBag**

Les deux `ViewData` et `ViewBag` sont également des approches valides pour le passage de petites quantités de données entre les contrôleurs et les vues. Le choix de l’application à utiliser est selon la préférence. Vous pouvez combiner des `ViewData` et `ViewBag` objets, toutefois, le code est plus facile à lire et à maintenir avec une approche utilisée régulièrement. Les deux approches sont résolues dynamiquement lors de l’exécution et donc enclin à provoquer des erreurs d’exécution. Certaines équipes de développement de les évitent.

### <a name="dynamic-views"></a>Vues dynamiques

Les vues qui ne déclarent pas un modèle de type à l’aide de `@model` mais qui ont passé à une instance de modèle (par exemple, `return View(Address);`) peuvent référencer dynamiquement les propriétés de l’instance :

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Cette fonctionnalité offre une souplesse, mais ne propose pas de protection de la compilation ou d’IntelliSense. Si la propriété n’existe pas, la génération de la page Web échoue lors de l’exécution.

## <a name="more-view-features"></a>D’autres fonctionnalités d’affichage

[Les tag helpers](xref:mvc/views/tag-helpers/intro) rendent facile l'ajout de comportement côté serveur à des balises HTML existantes. Les tag helpers permettent d'éviter d'écrire du code personnalisé ou des helpers dans vos vues. Les tag helpers sont appliqués en tant qu’attributs aux éléments HTML et sont ignorés par les éditeurs qui ne peuvent pas les traiter. Cela vous permet de modifier et de restituer le balisage de vue dans une variété d’outils.

Générer un balisage HTML personnalisé peut être obtenu avec de nombreux helpers HTML intégrés. Une logique plus complexe de l’interface utilisateur peut être gérée par [affichage des composants](xref:mvc/views/view-components). Les composants de vue fournissent la même séparation de responsabilité (SoC) que les contrôleurs et les vues offrent. Il peuvent éliminer le besoin d'utiliser des actions ou des vues qui traitent les données utilisées par les éléments d’interface couramment utilisés.

Comme de nombreux aspects d’ASP.NET Core, les vues prennent en charge [injection de dépendance](xref:fundamentals/dependency-injection), autorisant ainsi que les services soient [injectées dans les vues](xref:mvc/views/dependency-injection).
