---
title: Présentation des pages Razor dans ASP.NET Core
author: Rick-Anderson
description: Découvrez comment les pages Razor dans ASP.NET Core permettent de développer des scénarios orientés page de façon plus simple et plus productive qu’avec MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 406e89c96ea63493091d0287077e244faee5f730
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308005"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Présentation des pages Razor dans ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT) et [Ryan Nowak](https://github.com/rynowak)

Les pages Razor constituent un nouvel aspect d’ASP.NET Core MVC qui permet de développer des scénarios orientés page de façon plus simple et plus productive.

Si vous cherchez un didacticiel qui utilise l’approche Model-View-Controller, consultez [Bien démarrer avec ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

Ce document fournit une introduction aux pages Razor. Il ne s’agit pas d’un didacticiel pas à pas. Si certaines sections vous semblent trop techniques, consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start). Pour une vue d’ensemble d’ASP.NET Core, consultez [Introduction à ASP.NET Core](xref:index).

## <a name="prerequisites"></a>Prérequis

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>Créer un projet Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Pour obtenir des instructions sur la création d’un projet Razor Pages, consultez [Bien démarrer avec Razor Pages](xref:tutorials/razor-pages/razor-pages-start).

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio pour Mac](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Exécutez `dotnet new webapp` à partir de la ligne de commande.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Exécutez `dotnet new razor` à partir de la ligne de commande.

::: moniker-end

Ouvrez le fichier *.csproj* généré à partir de Visual Studio pour Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

Exécutez `dotnet new webapp` à partir de la ligne de commande.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Exécutez `dotnet new razor` à partir de la ligne de commande.

::: moniker-end

---

## <a name="razor-pages"></a>Pages Razor

La fonctionnalité Pages Razor est activée dans *Startup.cs* :

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Considérez une page de base : <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Le code précédent ressemble beaucoup à un [fichier vue Razor](xref:tutorials/first-mvc-app/adding-view) utilisé dans une application ASP.NET Core avec des contrôleurs et des vues. Ce qui le rend différent est la directive `@page`. `@page` fait du fichier une action MVC, ce qui signifie qu’il gère les demandes directement, sans passer par un contrôleur. `@page` doit être la première directive Razor sur une page. `@page` affecte le comportement d’autres constructions Razor.

Une page similaire, utilisant une classe `PageModel`, est illustrée dans les deux fichiers suivants. Le fichier *Pages/Index2.cshtml* :

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Le modèle de page *Pages/Index2.cshtml.cs* :

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Par convention, le fichier de classe `PageModel` a le même nom que le fichier Page Razor, avec *.cs* en plus. Par exemple, la page Razor précédente est *Pages/Index2.cshtml*. Le fichier contenant la classe `PageModel` se nomme *Pages/Index2.cshtml.cs*.

Les associations des chemins d’URL aux pages sont déterminées par l’emplacement de la page dans le système de fichiers. Le tableau suivant montre un chemin Page Razor et l’URL correspondante :

| Nom et chemin de fichier               | URL correspondante |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` ou `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` ou `/Store/Index` |

Remarques :

* Le runtime recherche les fichiers Pages Razor dans le dossier *Pages* par défaut.
* `Index` est la page par défaut quand une URL n’inclut pas de page.

## <a name="write-a-basic-form"></a>Écrire un formulaire de base

Les pages Razor sont conçues pour que les modèles courants utilisés avec les navigateurs web soient faciles à implémenter lors de la création d’une application. La [liaison de modèle](xref:mvc/models/model-binding), les [Tag Helpers](xref:mvc/views/tag-helpers/intro) et les assistances HTML *fonctionnent tous* avec les propriétés définies dans une classe Page Razor. Considérez une page qui implémente un formulaire « Nous contacter » de base pour le modèle `Contact` :

Pour les exemples de ce document, le `DbContext` est initialisé dans le fichier [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Le modèle de données :

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Le contexte de la base de données :

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

Le fichier vue *Pages/Create.cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Le modèle de page *Pages/Create.cshtml.cs* :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Par convention, la classe `PageModel` se nomme `<PageName>Model` et se trouve dans le même espace de noms que la page.

La classe `PageModel` permet de séparer la logique d’une page de sa présentation. Elle définit des gestionnaires de page pour les demandes envoyées à la page et les données utilisées pour l’afficher. Cette séparation permet de gérer les dépendances entre les pages au moyen de [l’injection de dépendances](xref:fundamentals/dependency-injection) et d’effectuer des [tests unitaires](xref:test/razor-pages-tests) sur les pages.

La page a une *méthode de gestionnaire* `OnPostAsync`, qui s’exécute sur les requêtes `POST` (quand un utilisateur poste le formulaire). Vous pouvez ajouter des méthodes de gestionnaire pour n’importe quel verbe HTTP. Les gestionnaires les plus courants sont :

* `OnGet` pour initialiser l’état nécessaire pour la page. Exemple [OnGet](#OnGet).
* `OnPost` pour gérer les envois de formulaire.

Le suffixe de nommage `Async` est facultatif, mais souvent utilisé par convention pour les fonctions asynchrones. Le code `OnPostAsync` dans l’exemple précédent est similaire à celui que vous écririez normalement dans un contrôleur. Le code précédent est typique des pages Razor. La plupart des primitives MVC comme la [liaison de modèle](xref:mvc/models/model-binding), la [validation](xref:mvc/models/validation) et les résultats d’action sont partagées.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

La méthode `OnPostAsync` précédente :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Le flux de base de `OnPostAsync` :

Recherchez les erreurs de validation.

* S’il n’y a aucune erreur, enregistrez les données et redirigez.
* S’il y a des erreurs, réaffichez la page avec des messages de validation. La validation côté client est identique à celle des applications ASP.NET Core MVC traditionnelles. Dans de nombreux cas, les erreurs de validation sont détectées sur le client et jamais envoyées au serveur.

Quand les données sont entrées correctement, la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `RedirectToPage` pour retourner une instance de `RedirectToPageResult`. `RedirectToPage` est un nouveau résultat d’action, semblable à `RedirectToAction` ou `RedirectToRoute`, mais personnalisé pour les pages. Dans l’exemple précédent, il redirige vers la page Index racine (`/Index`). `RedirectToPage` est détaillé dans la section [Génération d’URL pour les pages](#url_gen).

Quand le formulaire envoyé comporte des erreurs de validation (qui sont passées au serveur), la méthode de gestionnaire `OnPostAsync` appelle la méthode d’assistance `Page`. `Page` retourne une instance de `PageResult`. Le retour de `Page` est similaire à la façon dont les actions dans les contrôleurs retournent `View`. `PageResult` est le type de retour par défaut <!-- Review  --> pour une méthode de gestionnaire. Une méthode de gestionnaire qui retourne `void` restitue la page.

La propriété `Customer` utilise l’attribut `[BindProperty]` pour accepter la liaison de modèle.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Par défaut, Razor Pages lie les propriétés seulement avec des verbes non-`GET`. La liaison aux propriétés peut réduire la quantité de code à écrire. Elle réduit la quantité de code en utilisant la même propriété pour afficher les champs de formulaire (`<input asp-for="Customer.Name">`) et accepter l’entrée.

[!INCLUDE[](~/includes/bind-get.md)]

La page d’accueil (*Index.cshtml*) :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

La classe `PageModel` associée (*Index.cshtml.cs*) :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

Le fichier *Index.cshtml* contient le balisage suivant pour créer un lien d’édition pour chaque contact :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

Le [tag helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) a utilisé l’attribut `asp-route-{value}` pour générer un lien vers la page Edit. Le lien contient des données d’itinéraire avec l’ID de contact. Par exemple, `http://localhost:5000/Edit/1`. Utilisez l’attribut `asp-area` pour spécifier une zone. Pour plus d’informations, consultez <xref:mvc/controllers/areas>.

Le fichier *Pages/Edit.cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

La première ligne contient la directive `@page "{id:int}"`. La contrainte de routage `"{id:int}"` indique à la page qu’elle doit accepter les requêtes qui contiennent des données d’itinéraire `int`. Si une requête à la page ne contient de données d’itinéraire qui peuvent être converties en `int`, le runtime retourne une erreur HTTP 404 (introuvable). Pour que l’ID soit facultatif, ajoutez `?` à la contrainte de route :

 ```cshtml
@page "{id:int?}"
```

Le fichier *Pages/Edit.cshtml.cs* :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

Le fichier *Index.cshtml* contient également le balisage pour créer un bouton Supprimer pour chaque contact client :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Lorsque le bouton Supprimer est rendu en HTML, son `formaction` comprend des paramètres pour :

* L’ID du contact client spécifié par l’attribut `asp-route-id`.
* Le `handler` spécifié par l’attribut `asp-page-handler`.

Voici un exemple de bouton Supprimer rendu avec un ID de contact client de `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Quand le bouton est sélectionné, une demande `POST` de formulaire est envoyée au serveur. Par convention, le nom de la méthode de gestionnaire est sélectionné en fonction de la valeur du paramètre `handler` conformément au schéma `OnPost[handler]Async`.

Étant donné que le `handler` est `delete` dans cet exemple, la méthode de gestionnaire `OnPostDeleteAsync` est utilisée pour traiter la demande `POST`. Si `asp-page-handler` est défini sur une autre valeur, comme `remove`, une méthode de gestionnaire avec le nom `OnPostRemoveAsync` est sélectionnée.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

La méthode `OnPostDeleteAsync` :

* Accepte l’`id` de la chaîne de requête.
* Interroge la base de données pour le contact client avec `FindAsync`.
* Si le contact client est trouvé, il est supprimé de la liste des contacts client. La base de données est mise à jour.
* Appelle `RedirectToPage` pour rediriger vers la page Index racine (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a>Marquer les propriétés de page comme Required

Les propriétés définies sur `PageModel` peuvent être décorées avec l’attribut [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) :

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Pour plus d’informations, consultez [Validation de modèle](xref:mvc/models/validation).

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>Gérer les requêtes HEAD avec un gestionnaire OnGet de secours

Les requêtes `HEAD` vous permettent de récupérer les en-têtes pour une ressource spécifique. Contrairement aux requêtes `GET`, les requêtes `HEAD` ne retournent pas un corps de réponse.

En règle générale, un gestionnaire `OnHead` est créé et appelé pour les requêtes `HEAD` : 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Dans ASP.NET Core 2.1 ou ultérieur, Razor Pages se rabat sur un appel du gestionnaire `OnGet` si aucun gestionnaire `OnHead` n’est défini. Ce comportement est activé par l’appel à [SetCompatibilityVersion](xref:mvc/compatibility-version) dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

Les modèles par défaut génèrent l'appel `SetCompatibilityVersion` dans ASP.NET Core 2.1 et 2.2. `SetCompatibilityVersion` définit efficacement l’option Pages Razor `AllowMappingHeadRequestsToGetHandler` sur `true`.

Au lieu d’adhérer à tous les comportements avec `SetCompatibilityVersion`, vous pouvez adhérer explicitement à des comportements *spécifiques*. Le code suivant adhère à l’autorisation de mappage des requêtes `HEAD` au gestionnaire `OnGet` :

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF et pages Razor

Vous n’avez aucun code à écrire pour la [validation anti-contrefaçon](xref:security/anti-request-forgery). La validation et la génération de jetons anti-contrefaçon sont automatiquement incluses dans les pages Razor.

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Utilisation de dispositions, partiels, modèles et Tag Helpers avec les pages Razor

Les pages Razor fonctionnent avec toutes les fonctionnalités du moteur de vue Razor. Les dispositions, partiels, modèles, Tag Helpers, *_ViewStart.cshtml* et *_ViewImports.cshtml* fonctionnent de la même façon que pour les vues Razor classiques.

Nous allons nettoyer un peu cette page en tirant parti de certaines de ces fonctionnalités.

::: moniker range=">= aspnetcore-2.1"

Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/Shared/_Layout.cshtml* :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ajoutez une [page de disposition](xref:mvc/views/layout) à *Pages/_Layout.cshtml* :

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

La [disposition](xref:mvc/views/layout) :

* Contrôle la disposition de chaque page (à moins que la page ne refuse la disposition).
* Importe des structures HTML telles que JavaScript et les feuilles de style.

Pour plus d’informations, consultez [Page de disposition](xref:mvc/views/layout).

La propriété [Layout](xref:mvc/views/layout#specifying-a-layout) est définie dans *Pages/_ViewStart.cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

La disposition est dans le dossier *Pages/Shared*. Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active. Une disposition dans le dossier *Pages/Shared* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.

Le fichier de disposition doit être placé dans le dossier *Pages/Shared*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

La disposition est dans le dossier *Pages*. Les pages recherchent d’autres vues (dispositions, modèles, partiels) hiérarchiquement, en commençant dans le même dossier que la page active. Une disposition dans le dossier *Pages* peut être utilisée à partir de n’importe quelle page Razor sous le dossier *Pages*.

::: moniker-end

Nous vous recommandons de **ne pas** placer le fichier de disposition dans le dossier *Views/Shared*. *Views/Shared* est un modèle de vues MVC. Les pages Razor sont censées s’appuyer sur la hiérarchie des dossiers, pas sur les conventions de chemins.

La recherche de vue à partir d’une page Razor inclut le dossier *Pages*. Les dispositions, modèles et partiels que vous utilisez avec les contrôleurs MVC et les vues Razor conventionnelles *fonctionnent simplement*.

Ajoutez un fichier *Pages/_ViewImports.cshtml* :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` est expliqué plus loin dans le didacticiel. La directive `@addTagHelper` permet de bénéficier des [Tag Helpers intégrés](xref:mvc/views/tag-helpers/builtin-th/Index) dans toutes les pages du dossier *Pages*.

<a name="namespace"></a>

Quand la directive `@namespace` est utilisée explicitement sur une page :

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

La directive définit l’espace de noms pour la page. La directive `@model` n’a pas besoin d’inclure l’espace de noms.

Quand la directive `@namespace` est contenue dans *_ViewImports.cshtml*, l’espace de noms spécifié fournit le préfixe de l’espace de noms généré dans la Page qui importe la directive `@namespace`. Le reste de l’espace de noms généré (la partie suffixe) est le chemin relatif séparé par un point entre le dossier contenant *_ViewImports.cshtml* et le dossier contenant la page.

Par exemple, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* définit explicitement l’espace de noms :

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

Le fichier *Pages/_ViewImports.cshtml* définit l’espace de noms suivant :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

L’espace de noms généré pour la page Razor *Pages/Customers/Edit.cshtml* est identique à la classe `PageModel`.

`@namespace` *fonctionne également avec les vues Razor classiques*.

Le fichier de vue *Pages/Create.cshtml* d’origine :

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Le fichier vue *Pages/Create.cshtml* mis à jour :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

Le [projet de démarrage de pages Razor](#rpvs17) contient *Pages/_ValidationScriptsPartial.cshtml*, qui connecte la validation côté client.

Pour plus d'informations sur les affichages partiels, consultez <xref:mvc/views/partial>.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Génération d’URL pour les pages

La page `Create`, illustrée précédemment, utilise `RedirectToPage` :

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

L’application a la structure de fichiers/dossiers suivante :

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

Une fois l’opération réussie, les pages *Pages/Customers/Create.cshtml* et *Pages/Customers/Edit.cshtml* redirigent vers *Pages/Index.cshtml*. La chaîne `/Index` fait partie de l’URI pour accéder à la page précédente. La chaîne `/Index` peut être utilisée pour générer l’URI de la page *Pages/Index.cshtml*. Par exemple :

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Le nom de la page est le chemin de la page à partir du dossier racine */Pages* avec un `/` devant (par exemple, `/Index`). Les exemples de génération d’URL précédents offrent des options améliorées et des capacités fonctionnelles sur le codage en dur d’une URL. La génération d’URL utilise le [routage](xref:mvc/controllers/routing) et peut générer et encoder des paramètres en fonction de la façon dont l’itinéraire est défini dans le chemin de destination.

La génération d’URL pour les pages prend en charge les noms relatifs. Le tableau suivant montre la page Index sélectionnée avec différents paramètres `RedirectToPage` à partir de *Pages/Customers/Create.cshtml* :

| RedirectToPage(x)| Page |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")` et `RedirectToPage("../Index")` sont des *noms relatifs*. Le paramètre `RedirectToPage` est *combiné* avec le chemin de la page active pour calculer le nom de la page de destination.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

La liaison de nom relatif est utile lors de la création de sites avec une structure complexe. Si vous utilisez des noms relatifs pour établir une liaison entre les pages d’un dossier, vous pouvez renommer ce dossier. Tous les liens fonctionneront encore (car ils n’incluent pas le nom du dossier).

::: moniker range=">= aspnetcore-2.1"

Pour rediriger vers une page située dans une autre [Zone](xref:mvc/controllers/areas), spécifiez la zone :

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

Pour plus d’informations, consultez <xref:mvc/controllers/areas>.

## <a name="viewdata-attribute"></a>Attribut ViewData

Les données peuvent être passées à une page avec [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Les valeurs des propriétés définies sur des contrôleurs ou sur des modèles de page Razor décorés avec `[ViewData]` sont stockées et chargées à partir de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

Dans l’exemple suivant, `AboutModel` contient une propriété `Title` décorée avec `[ViewData]`. La propriété `Title` a pour valeur le titre de la page À propos de :

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

Dans la page À propos de, accédez à la propriété `Title` en tant que propriété de modèle :

```cshtml
<h1>@Model.Title</h1>
```

Dans la disposition, le titre est lu à partir du dictionnaire ViewData :

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core expose la propriété [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) sur un [contrôleur](/dotnet/api/microsoft.aspnetcore.mvc.controller). Cette propriété stocke les données jusqu’à ce qu’elles soient lues. Vous pouvez utiliser les méthodes `Keep` et `Peek` pour examiner les données sans suppression. `TempData` est utile pour la redirection, quand des données sont nécessaires pour plusieurs requêtes.

L’attribut `[TempData]` est une nouveauté dans ASP.NET Core 2.0. Il est pris en charge sur les contrôleurs et les pages.

Le code suivant définit la valeur de `Message` à l’aide de `TempData` :

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Le balisage suivant dans le fichier *Pages/Customers/Index.cshtml* affiche la valeur de `Message` à l’aide de `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

Le modèle de page *Pages/Customers/Index.cshtml.cs* applique l’attribut `[TempData]` à la propriété `Message`.

```cs
[TempData]
public string Message { get; set; }
```

Pour plus d’informations, consultez [TempData](xref:fundamentals/app-state#tempdata).

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>Plusieurs gestionnaires par page

La page suivante génère un balisage pour deux gestionnaires en utilisant le Tag Helper `asp-page-handler` :

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Le formulaire dans l’exemple précédent contient deux boutons d’envoi, chacun utilisant `FormActionTagHelper` pour envoyer à une URL différente. L’attribut `asp-page-handler` est un complément de `asp-page`. `asp-page-handler` génère des URL qui envoient à chacune des méthodes de gestionnaire définies par une page. `asp-page` n’est pas spécifié, car l’exemple établit une liaison à la page active.

Le modèle de page :

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Le code précédent utilise des *méthodes de gestionnaire nommées*. Pour créer des méthodes de gestionnaire nommées, il faut prendre le texte dans le nom après `On<HTTP Verb>` et avant `Async` (le cas échéant). Dans l’exemple précédent, les méthodes de page sont OnPost**JoinList**Async et OnPost**JoinListUC**Async. Avec *OnPost* et *Async* supprimés, les noms des gestionnaires sont `JoinList` et `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="custom-routes"></a>Routes personnalisées

Utilisez la directive `@page` pour :

* Spécifier une route personnalisée vers une page. Par exemple, la route vers la page À propos peut être définie sur `/Some/Other/Path` avec `@page "/Some/Other/Path"`.
* Ajouter des segments à la route par défaut d’une page. Par exemple, un segment « item » peut être ajouté à la route par défaut d’une page avec `@page "item"`.
* Ajouter des paramètres à la route par défaut d’une page. Par exemple, un paramètre d’ID, `id`, peut être nécessaire pour une page avec `@page "{id}"`.

Un chemin relatif racine désigné par un tilde (`~`) au début du chemin est pris en charge. Par exemple, `@page "~/Some/Other/Path"` est identique à `@page "/Some/Other/Path"`.

Vous pouvez remplacer la chaîne de requête `?handler=JoinList` dans l’URL par un segment de routage `/JoinList` en spécifiant le modèle de routage `@page "{handler?}"`.

Si vous ne voulez pas avoir la chaîne de requête `?handler=JoinList` dans l’URL, vous pouvez changer l’itinéraire pour placer le nom du gestionnaire dans la partie chemin de l’URL. Vous pouvez personnaliser l’itinéraire en ajoutant un modèle d’itinéraire placé entre des guillemets doubles après la directive `@page`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Avec le code précédent, le chemin d’URL qui envoie à `OnPostJoinListAsync` est `http://localhost:5000/Customers/CreateFATH/JoinList`. Le chemin d’URL qui envoie à `OnPostJoinListUCAsync` est `http://localhost:5000/Customers/CreateFATH/JoinListUC`.

Le `?` suivant `handler` signifie que le paramètre d’itinéraire est facultatif.

## <a name="configuration-and-settings"></a>Configuration et paramètres

Pour configurer les options avancées, utilisez la méthode d’extension `AddRazorPagesOptions` sur le générateur MVC :

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Actuellement, vous pouvez utiliser `RazorPagesOptions` pour définir le répertoire racine pour les pages, ou ajouter des conventions de modèle d’application pour les pages. Nous permettrons à l’avenir une plus grande extensibilité en ce sens.

Pour précompiler des vues, consultez [Compilation de vue Razor](xref:mvc/views/view-compilation).

[Téléchargez ou affichez des exemples de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).

Consultez [Bien démarrer avec les pages Razor](xref:tutorials/razor-pages/razor-pages-start) qui s’appuie sur cette introduction.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Spécifier que les pages Razor se trouvent à la racine du contenu

Par défaut, les pages Razor sont associées à la racine */Pages*. Ajoutez [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent à la racine de contenu ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de l’application :

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Spécifier que les pages Razor se trouvent dans un répertoire racine personnalisé

Ajoutez [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) pour spécifier que vos pages Razor se trouvent dans le répertoire racine personnalisé de l’application (fournissez un chemin relatif) :

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
