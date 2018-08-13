---
title: Vues partielles dans ASP.NET Core
author: ardalis
description: Découvrez ce qu’est une vue partielle, une vue rendue dans une autre vue, et quand l’utiliser dans les applications ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 2223f3c6e42927def4b91ff9da58c228e5904756
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655321"
---
# <a name="partial-views-in-aspnet-core"></a>Vues partielles dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) et [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core prend en charge les vues partielles. Les vues partielles sont utilisées pour partager des parties réutilisables de pages web entre différentes vues.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Que sont les vues partielles ?

Une vue partielle est une vue affichée dans une autre vue. La sortie HTML générée par l’exécution de la vue partielle est affichée dans la vue appelante (ou parente). Tout comme les vues, les vues partielles utilisent l’extension de fichier *.cshtml*.

Par exemple, le modèle de projet **Application web** ASP.NET Core 2.1 inclut une vue partielle *_CookieConsentPartial.cshtml*. La vue partielle est chargée à partir de *_Layout.cshtml* :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Quand utiliser des vues partielles ?

Les vues partielles sont un moyen efficace de découper des vues volumineuses en composants plus petits. Elles permettent de réduire la duplication du contenu d’une vue et de réutiliser les éléments des vues. Les éléments de disposition communs doivent être spécifiés dans [_Layout.cshtml](xref:mvc/views/layout). Le contenu réutilisable non lié à une disposition peut être encapsulé dans des vues partielles.

Dans une page complexe formée de plusieurs parties logiques, il peut s’avérer utile de travailler sur chaque partie en tant que vue partielle. Chaque partie de la page peut être visualisée de façon isolée par rapport au reste de la page. La vue de la page elle-même devient plus simple, puisqu’elle contient uniquement la structure de page globale et les appels permettant d’afficher les vues partielles.

Les contrôleurs ASP.NET Core MVC ont une méthode [PartialView](/dotnet/api/microsoft.aspnetcore.mvc.controller.partialview#Microsoft_AspNetCore_Mvc_Controller_PartialView) qui est appelée à partir d’une méthode d’action. Les Razor Pages n’ont aucune méthode `PartialView` équivalente sur [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).

## <a name="declare-partial-views"></a>Déclarer des vues partielles

Vous créez les vues partielles comme les autres vues : en créant un fichier *.cshtml* dans le dossier &mdash;Vues *. Il n’existe aucune différence sémantique entre une vue partielle et une vue classique. Cependant, leur rendu est différent. Une vue peut être retournée directement à partir du [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) d’un contrôleur, et cette même vue peut être utilisée comme vue partielle. La principale différence entre le rendu d’une vue et celui d’une vue partielle est que les vues partielles n’exécutent pas *_ViewStart.cshtml*. Les vues standard exécutent *_ViewStart.cshtml*. Découvrez-en plus sur *_ViewStart.cshtml* dans [Disposition](xref:mvc/views/layout)).

Par convention, les noms de fichiers des vues partielles commencent souvent par `_`. Cette convention de nommage n’est pas une condition requise, mais elle aide à différencier visuellement les vues partielles des vues standard.

## <a name="reference-a-partial-view"></a>Référencer une vue partielle

Au sein d’une page de vue, il existe plusieurs façons d’afficher une vue partielle. La bonne pratique consiste à utiliser le rendu asynchrone.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Tag Helper Partial

Le Tag Helper Partial nécessite ASP.NET Core 2.1 ou ultérieur. Il effectue un rendu asynchrone et utilise une syntaxe de type HTML :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Pour plus d'informations, consultez <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Assistance HTML asynchrone

Quand vous utilisez une assistance HTML, la bonne pratique consiste à utiliser [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Elle retourne un type [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) wrappé dans un `Task`. La méthode est référencée en ajoutant le préfixe `@` à l’appel :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

En guise d’alternative, vous pouvez afficher une vue partielle avec [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Cette méthode ne retourne aucun résultat. Elle envoie la sortie rendue directement à la réponse. Comme elle ne retourne aucun résultat, cette méthode doit être appelée dans un bloc de code Razor :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Dans la mesure où elle envoie directement le résultat, `RenderPartialAsync` peut être plus efficace dans certains scénarios. Toutefois, nous vous recommandons d’utiliser `PartialAsync`.

### <a name="synchronous-html-helper"></a>Assistance HTML synchrone

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) et [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) sont les équivalents synchrones de `PartialAsync` et `RenderPartialAsync`, respectivement. L’utilisation des équivalents synchrones n’est pas recommandée en raison du risque d’interblocage dans certains scénarios. Les versions ultérieures ne contiendront pas les méthodes synchrones.

> [!IMPORTANT]
> Si vos vues doivent exécuter du code, utilisez un [composant de vue](xref:mvc/views/view-components) au lieu d’une vue partielle.

::: moniker range=">= aspnetcore-2.1"

Dans ASP.NET Core 2.1 ou ultérieur, l’appel de `Partial` ou `RenderPartial` génère un avertissement de l’analyseur. Par exemple, l’utilisation de `Partial` génère le message d’avertissement suivant :

> L’utilisation de IHtmlHelper.Partial peut entraîner des interblocages d’application. Utilisez plutôt le Tag Helper `<partial>` ou `IHtmlHelper.PartialAsync`.

Remplacez les appels à `@Html.Partial` par `@await Html.PartialAsync` ou le Tag Helper Partial. Pour plus d’informations sur la migration du Tag Helper Partial, consultez [Migrer à partir d’une assistance HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Découverte des vues partielles

Quand vous référencez une vue partielle, vous pouvez faire référence à son emplacement de plusieurs façons. Exemple :

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

L’exemple précédent utilise le Tag Helper Partial, qui nécessite ASP.NET Core 2.1 ou ultérieur. L’exemple suivant utilise des helpers HTML asynchrones pour accomplir la même tâche.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Vous pouvez avoir différentes vues partielles portant le même nom de fichier dans des dossiers de vues distincts. Quand vous référencez des vues par leur nom (sans extension de fichier), les vues de chaque dossier utilisent la vue partielle située dans le même dossier. Vous pouvez également spécifier une vue partielle par défaut à utiliser, en la plaçant dans le dossier *Partagé*. La vue partielle partagée est utilisée par toutes les vues qui n’ont pas leur propre version de la vue partielle. Une vue partielle par défaut (dans *Partagé*) peut être remplacée par une vue partielle ayant le même nom dans le dossier de la vue parente.

Une vue partielle peut être *chaînée*, c’est-à-dire appeler une autre vue partielle (tant que vous ne créez pas de boucle). Dans chaque vue ou vue partielle, les chemins relatifs sont toujours relatifs à cette vue, et non à la vue racine ou parente.

> [!NOTE]
> Une `section` [Razor](xref:mvc/views/razor) définie dans une vue partielle est invisible aux vues parentes. La `section` est visible uniquement par la vue partielle dans laquelle elle est définie.

## <a name="access-data-from-partial-views"></a>Accéder à des données à partir de vues partielles

Quand une vue partielle est instanciée, elle obtient une copie du dictionnaire de `ViewData` de la vue parente. Les mises à jour apportées aux données de la vue partielle ne sont pas conservées dans la vue parente. Tout changement apporté à `ViewData` dans une vue partielle est perdu quand la vue partielle est retournée.

Vous pouvez passer une instance de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) à la vue partielle :

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Vous pouvez passer un modèle dans une vue partielle. Ce modèle peut être le modèle de vue de la page ou un objet personnalisé. Vous pouvez passer un modèle à `PartialAsync` ou `RenderPartialAsync` :

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Vous pouvez passer une instance de `ViewDataDictionary` et un modèle de vue à une vue partielle :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Le code ci-dessous montre la vue *Views/Articles/Read.cshtml*, qui contient deux vues partielles. La seconde vue partielle passe un modèle et `ViewData` à la vue partielle. Utilisez la surcharge de constructeur `ViewDataDictionary` mise en surbrillance pour passer un nouveau dictionnaire `ViewData` tout en conservant le dictionnaire `ViewData` existant.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial* :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

Le *_ArticleSection* partiel :

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Au moment de l’exécution, les vues partielles sont affichées dans la vue parente, elle-même affichée dans le fichier *_Layout.cshtml* partagé.

![sortie de vue partielle](partial/_static/output.png)

## <a name="additional-resources"></a>Ressources supplémentaires

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
