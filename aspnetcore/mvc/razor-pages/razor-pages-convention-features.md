---
title: "Fonctionnalités de convention de routage et d’application pour les pages Razor dans ASP.NET Core"
author: guardrex
description: "Découvrez comment les fonctionnalités de convention du fournisseur de modèles de routages et d’applications vous permettent de gérer le routage, la découverte et le traitement des pages."
manager: wpickett
ms.author: riande
ms.date: 10/23/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/razor-pages/razor-pages-convention-features
ms.openlocfilehash: bf1c895fc972310d5541d0098226d58b8183e320
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-route-and-app-convention-features-in-aspnet-core"></a>Fonctionnalités de convention de routage et d’application pour les pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Découvrez comment utiliser les fonctionnalités de convention du fournisseur de modèles de routages et d’applications pour contrôler le routage, la découverte et le traitement des pages dans les applications à base de pages Razor. Quand vous devez configurer des routages de pages personnalisés pour des pages individuelles, configurez le routage vers les pages avec la convention [AddPageRoute](#configure-a-page-route) décrite plus loin dans cette rubrique.

Utilisez l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/razor-pages-convention-features/sample) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) pour explorer les fonctionnalités décrites dans cette rubrique.

| Fonctionnalités | L’exemple montre... |
| -------- | --------------------------- |
| [Conventions de modèles de routages et d’applications](#add-route-and-app-model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Ajout d’un modèle de routage et d’un en-tête aux pages d’une application. |
| [Conventions d’actions de routage de pages](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Ajout d’un modèle de routage aux pages d’un dossier et à une page unique. |
| [Conventions d’actions de modèle de page](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe de filtre, expression lambda ou fabrique de filtres)</li></ul> | Ajout d’un en-tête aux pages d’un dossier, ajout d’un en-tête à une page unique et configuration d’une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) pour ajouter un en-tête aux pages d’une application. |
| [Fournisseur de modèles d’applications de pages par défaut](#replace-the-default-page-app-model-provider) | Remplacement du fournisseur de modèles de pages par défaut pour changer les conventions de nommage du gestionnaire. |

## <a name="add-route-and-app-model-conventions"></a>Ajouter des conventions de modèles de routages et d’applications

Ajoutez un délégué pour [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) afin d’ajouter des conventions de modèles de routages et d’applications qui s’appliquent aux pages Razor.

**Ajouter une convention de modèle de routage à toutes les pages**

Utilisez [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) à la collection des instances de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) qui sont appliquées durant la construction d’un modèle de routage et de page.

L’exemple d’application ajoute un modèle de routage `{globalTemplate?}` à toutes les pages de l’application :

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

> [!NOTE]
> La propriété `Order` de `AttributeRouteModel` a la valeur `0` (zéro). Cela permet de garantir que ce modèle est prioritaire pour la première position de la valeur des données de routage quand une valeur de routage unique est fournie. Par exemple, un modèle de routage `{aboutTemplate?}` est ajouté plus loin dans cette rubrique. Le modèle `{aboutTemplate?}` reçoit l’ordre `Order` avec la valeur `1`. Quand la page About est demandée sur `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 0`) et non `RouteData.Values["aboutTemplate"]` (`Order = 1`) en raison de la définition de la propriété `Order`.

*Startup.cs* :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet1)]

Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue` et examinez le résultat :

![La page About est demandée avec un segment de routage pour GlobalRouteValue. La page affichée montre que la valeur des données de routage est capturée dans la méthode OnGet de la page.](razor-pages-convention-features/_static/about-page-global-template.png)

**Ajouter une convention de modèle d’application à toutes les pages**

Utilisez [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) à la collection des instances de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) qui sont appliquées durant la construction d’un modèle de routage et de page.

Pour illustrer cette convention et bien d’autres, plus loin dans cette rubrique, l’exemple d’application inclut une classe `AddHeaderAttribute`. Le constructeur de classe accepte une chaîne `name` et un tableau de chaînes `values`. Ces valeurs sont utilisées dans sa méthode `OnResultExecuting` pour définir un en-tête de réponse. La classe complète est affichée dans la section [Conventions d’actions de modèle de page](#page-model-action-conventions), plus loin dans cette rubrique.

L’exemple d’application utilise la classe `AddHeaderAttribute` pour ajouter un en-tête, `GlobalHeader`, à toutes les pages de l’application :

[!code-csharp[Main](razor-pages-convention-features/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs* :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet2)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que GlobalHeader a été ajouté.](razor-pages-convention-features/_static/about-page-global-header.png)

## <a name="page-route-action-conventions"></a>Conventions d’actions de routage de pages

Le fournisseur de modèles de routages par défaut, qui dérive de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider), appelle des conventions conçues pour fournir des points d’extensibilité permettant de configurer les routages de pages.

**Convention de modèle de routage de dossier**

Utilisez [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) qui appelle une action sur le [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pour toutes les pages situées dans le dossier spécifié.

L’exemple d’application utilise `AddFolderRouteModelConvention` pour ajouter un modèle de routage `{otherPagesTemplate?}` aux pages du dossier *OtherPages* :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet3)]

> [!NOTE]
> La propriété `Order` de `AttributeRouteModel` a la valeur `1`. Cela permet de garantir que le modèle de `{globalTemplate?}` (défini plus tôt dans cette rubrique) est prioritaire pour la première position de la valeur des données de routage quand une valeur de routage unique est fournie. Si la page Page1 est demandée sur `/OtherPages/Page1/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 0`) et non `RouteData.Values["otherPagesTemplate"]` (`Order = 1`) en raison de la définition de la propriété `Order`.

Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` et examinez le résultat :

![La page Page1 du dossier OtherPages est demandée avec un segment de routage pour GlobalRouteValue et OtherPagesRouteValue. La page affichée montre que les valeurs des données de routage sont capturées dans la méthode OnGet de la page.](razor-pages-convention-features/_static/otherpages-page1-global-and-otherpages-templates.png)

**Convention de modèle de routage de page**

Utilisez [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) qui appelle une action sur le [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pour la page ayant le nom spécifié.

L’exemple d’application utilise `AddPageRouteModelConvention` pour ajouter un modèle de routage `{aboutTemplate?}` à la page About :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet4)]

> [!NOTE]
> La propriété `Order` de `AttributeRouteModel` a la valeur `1`. Cela permet de garantir que le modèle de `{globalTemplate?}` (défini plus tôt dans cette rubrique) est prioritaire pour la première position de la valeur des données de routage quand une valeur de routage unique est fournie. Si la page About est demandée sur `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 0`) et non `RouteData.Values["aboutTemplate"]` (`Order = 1`) en raison de la définition de la propriété `Order`.

Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue/AboutRouteValue` et examinez le résultat :

![La page About est demandée avec des segments de routage pour GlobalRouteValue et AboutRouteValue. La page affichée montre que les valeurs des données de routage sont capturées dans la méthode OnGet de la page.](razor-pages-convention-features/_static/about-page-global-and-about-templates.png)

## <a name="configure-a-page-route"></a>Configurer un routage de page

Utilisez [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) pour configurer un routage vers une page située dans le chemin spécifié. Les liens générés qui pointent vers la page utilisent le routage que vous avez spécifié. `AddPageRoute` utilise `AddPageRouteModelConvention` pour établir le routage.

L’exemple d’application crée un routage vers `/TheContactPage` pour *Contact.cshtml* :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet5)]

La page Contact est également accessible sur `/Contact` via son routage par défaut.

Le routage personnalisé de l’exemple d’application vers la page Contact permet un segment de routage `text` facultatif (`{text?}`). Cette page inclut également ce segment facultatif dans sa directive `@page`, au cas où le visiteur accèderait à la page via le routage `/Contact` :

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Contact.cshtml?highlight=1)]

Notez que l’URL générée pour le lien **Contact** dans la page affichée reflète le routage mis à jour :

![Lien Contact de l’exemple d’application dans la barre de navigation](razor-pages-convention-features/_static/contact-link.png)

![Le lien Contact dans la page HTML affichée indique que l’attribut href a la valeur ’/TheContactPage’](razor-pages-convention-features/_static/contact-link-source.png)

Visitez la page Contact via son routage ordinaire, `/Contact`, ou via le routage personnalisé, `/TheContactPage`. Si vous indiquez un segment de routage `text` supplémentaire, la page affiche le segment HTML que vous fournissez :

![Exemple dans le navigateur Edge d’un segment de routage ‘text’ facultatif fourni pour ‘TextValue’ dans l’URL. La page affichée montre la valeur du segment ‘text’.](razor-pages-convention-features/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Conventions d’actions de modèle de page

Le fournisseur de modèles de pages par défaut, qui implémente [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider), appelle des conventions conçues pour fournir des points d’extensibilité permettant de configurer les modèles de pages. Ces conventions sont utiles durant la génération et la modification des fonctionnalités de découverte et de traitement des pages.

Pour les exemples de cette section, l’exemple d’application utilise une classe `AddHeaderAttribute`, qui est un [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), et qui applique un en-tête de réponse :

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/AddHeader.cs?name=snippet1)]

À l’aide de conventions, l’exemple montre comment appliquer l’attribut à toutes les pages d’un dossier et à une seule page.

**Convention de modèle d’application de dossier**

Utilisez [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) qui appelle une action sur les instances de [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) pour toutes les pages situées dans le dossier spécifié.

L’exemple illustre l’utilisation de `AddFolderApplicationModelConvention` en ajoutant un en-tête, `OtherPagesHeader`, aux pages situées dans le dossier *OtherPages* de l’application :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet6)]

Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page OtherPages/Page1 montrent que OtherPagesHeader a été ajouté.](razor-pages-convention-features/_static/page1-otherpages-header.png)

**Convention de modèle d’application de page**

Utilisez [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) qui appelle une action sur le [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) pour la page ayant le nom spécifié.

L’exemple illustre l’utilisation de `AddPageApplicationModelConvention` en ajoutant un en-tête, `AboutHeader`, à la page About :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet7)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que AboutHeader a été ajouté.](razor-pages-convention-features/_static/about-page-about-header.png)

**Configurer un filtre**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configure le filtre spécifié à appliquer. Vous pouvez implémenter une classe de filtre. Toutefois, l’exemple d’application montre comment implémenter un filtre dans une expression lambda, laquelle est implémentée en arrière-plan en tant que fabrique qui retourne un filtre :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet8)]

Le modèle d’application de page est utilisé pour vérifier le chemin relatif des segments qui mènent à la page Page2 dans le dossier *OtherPages*. Si la condition est satisfaite, un en-tête est ajouté. Sinon, `EmptyFilter` est appliqué.

`EmptyFilter` est un [filtre d’action](xref:mvc/controllers/filters#action-filters). Dans la mesure où les filtres d’action sont ignorés par les pages Razor, `EmptyFilter` ne fait rien, comme cela est prévu, si le chemin ne contient pas `OtherPages/Page2`.

Demandez la page Page2 de l’exemple sur `localhost:5000/OtherPages/Page2` et examinez les en-têtes pour voir le résultat :

![OtherPagesPage2Header est ajouté à la réponse pour Page2.](razor-pages-convention-features/_static/page2-filter-header.png)

**Configurer une fabrique de filtres**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configure la fabrique spécifiée pour appliquer des [filtres](xref:mvc/controllers/filters) à toutes les pages Razor.

L’exemple d’application illustre l’utilisation d’une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) en ajoutant un en-tête, `FilterFactoryHeader`, et deux valeurs aux pages de l’application :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs* :

[!code-csharp[Main](razor-pages-convention-features/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que deux en-têtes FilterFactoryHeader ont été ajoutés.](razor-pages-convention-features/_static/about-page-filter-factory-header.png)

## <a name="replace-the-default-page-app-model-provider"></a>Remplacer le fournisseur de modèles d’applications de pages par défaut

Les pages Razor utilisent l’interface `IPageApplicationModelProvider` pour créer [DefaultPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider). Vous pouvez hériter du fournisseur de modèles par défaut afin de fournir votre propre logique d’implémentation pour la découverte et le traitement du gestionnaire. L’implémentation par défaut ([source de référence](https://github.com/aspnet/Mvc/blob/rel/2.0.1/src/Microsoft.AspNetCore.Mvc.RazorPages/Internal/DefaultPageApplicationModelProvider.cs)) établit des conventions pour le nommage de gestionnaire *sans nom* et *avec nom*, comme cela est décrit ci-dessous.

**Méthodes de gestionnaire sans nom par défaut**

Les méthodes de gestionnaire pour les verbes HTTP (méthodes de gestionnaire « sans nom ») suivent une convention : `On<HTTP verb>[Async]` (l’ajout de `Async` est facultatif mais il est recommandé pour les méthodes asynchrones).

| Méthode de gestionnaire sans nom     | Opération                      |
| -------------------------- | ------------------------------ |
| `OnGet`/`OnGetAsync`       | Initialise l’état de la page.     |
| `OnPost`/`OnPostAsync`     | Gère les requêtes POST.          |
| `OnDelete`/`OnDeleteAsync` | Gère les requêtes DELETE&#8224;. |
| `OnPut`/`OnPutAsync`       | Gère les requêtes PUT&#8224;.    |
| `OnPatch`/`OnPatchAsync`   | Gère les requêtes PATCH&#8224;.  |

&#8224;Permet d’effectuer des appels d’API à la page.

**Méthodes de gestionnaire avec nom par défaut**

Les méthodes de gestionnaire fournies par le développeur (méthodes de gestionnaire « avec nom ») suivent une convention similaire. Le nom du gestionnaire apparaît après le verbe HTTP ou entre le verbe HTTP et `Async` : `On<HTTP verb><handler name>[Async]` (l’ajout de `Async` est facultatif mais il est recommandé pour les méthodes asynchrones). Par exemple, les méthodes qui traitent les messages peuvent être nommées comme indiqué dans le tableau ci-dessous.

| Exemple de méthode de gestionnaire avec nom             | Exemple d’opération        |
| ---------------------------------------- | ------------------------ |
| `OnGetMessage`/`OnGetMessageAsync`       | Obtention d’un message.        |
| `OnPostMessage`/`OnPostMessageAsync`     | POST sur un message.          |
| `OnDeleteMessage`/`OnDeleteMessageAsync` | DELETE sur un message&#8224;. |
| `OnPutMessage`/`OnPutMessageAsync`       | PUT sur un message&#8224;.    |
| `OnPatchMessage`/`OnPatchMessageAsync`   | PATCH sur un message&#8224;.  |

&#8224;Permet d’effectuer des appels d’API à la page.

**Personnaliser les noms de méthodes de gestionnaire**

Supposons que vous souhaitiez changer le mode de nommage des méthodes de gestionnaire avec nom et sans nom. Il existe un autre schéma de nommage, qui consiste à éviter de commencer les noms de méthodes par « On » et à utiliser le premier segment de mot pour déterminer le verbe HTTP. Vous pouvez effectuer d’autres changements, par exemple convertir les verbes pour DELETE, PUT et de PATCH à POST. Avec un tel schéma, voici les noms de méthodes correspondants dans le tableau suivant.

| Méthode de gestionnaire                       | Opération                      |
| ------------------------------------ | ------------------------------ |
| `Get`                                | Initialise l’état de la page.     |
| `Post`/`PostAsync`                   | Gère les requêtes POST.          |
| `Delete`/`DeleteAsync`               | Gère les requêtes DELETE&#8224;. |
| `Put`/`PutAsync`                     | Gère les requêtes PUT&#8224;.    |
| `Patch`/`PatchAsync`                 | Gère les requêtes PATCH&#8224;.  |
| `GetMessage`                         | Obtention d’un message.              |
| `PostMessage`/`PostMessageAsync`     | POST sur un message.                |
| `DeleteMessage`/`DeleteMessageAsync` | POST sur un message à supprimer.      |
| `PutMessage`/`PutMessageAsync`       | POST sur un message à remplacer.         |
| `PatchMessage`/`PatchMessageAsync`   | POST sur un message à corriger.       |

&#8224;Permet d’effectuer des appels d’API à la page.

Pour établir ce schéma, effectuez l’héritage de la classe `DefaultPageApplicationModelProvider` et substituez la méthode [CreateHandlerModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.internal.defaultpageapplicationmodelprovider.createhandlermodel) afin de fournir une logique personnalisée pour la résolution des noms du gestionnaire [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). L’exemple d’application vous montre comment cela est effectué dans la classe `CustomPageApplicationModelProvider` :

[!code-csharp[Main](razor-pages-convention-features/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Caractéristiques principales de la classe :

* La classe hérite de `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` traite un gestionnaire pour identifier le verbe HTTP (`httpMethod`) et le nom de gestionnaire avec nom (`handlerName`) durant la création de `PageHandlerModel`.
  * Le suffixe `Async` est ignoré, s’il est présent.
  * La casse est utilisée pour analyser le verbe HTTP à partir du nom de la méthode.
  * Quand le nom de la méthode (sans `Async`) correspond au nom du verbe HTTP, il n’existe aucun gestionnaire nommé. `handlerName` a la valeur `null`, et le nom de la méthode est `Get`, `Post`, `Delete`, `Put` ou `Patch`.
  * Quand le nom de la méthode (sans `Async`) est plus long que le nom du verbe HTTP, il existe un gestionnaire nommé. La propriété `handlerName` a la valeur `<method name (less 'Async', if present)>`. Par exemple, « GetMessage » et « GetMessageAsync » génèrent le nom de gestionnaire « GetMessage ».
  * Les verbes DELETE, PUT et PATCH HTTP sont convertis en POST.

Inscrivez `CustomPageApplicationModelProvider` dans la classe `Startup` :

[!code-csharp[Main](razor-pages-convention-features/sample/Startup.cs?name=snippet10)]

Le modèle de page dans *Index.cshtml.cs* illustre la façon dont les conventions de nommage de méthode de gestionnaire ordinaire changent pour les pages de l’application. Le nommage classique à l’aide du préfixe « On » utilisé avec les pages Razor est supprimé. La méthode qui initialise l’état de la page se nomme désormais `Get`. Vous pouvez constater l’utilisation de cette convention dans l’ensemble de l’application, si vous ouvrez un modèle de page pour l’une des pages.

Chacune des autres méthodes commence par le verbe HTTP qui décrit son traitement. Les deux méthodes qui commencent par `Delete` sont traitées normalement comme des verbes HTTP DELETE, mais la logique dans `TryParseHandlerMethod` définit explicitement le verbe POST pour les deux gestionnaires.

Notez que `Async` est facultatif entre `DeleteAllMessages` et `DeleteMessageAsync`. Les deux méthodes sont asynchrones. Toutefois, vous pouvez choisir d’utiliser ou non le suffixe `Async`. Nous vous recommandons de le faire. `DeleteAllMessages` est utilisé ici à des fins de démonstration, mais nous vous recommandons de nommer cette méthode `DeleteAllMessagesAsync`. Cela n’affecte pas le traitement de l’implémentation de l’exemple. Toutefois, l’utilisation du suffixe `Async` indique qu’il s’agit d’une méthode asynchrone.

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Notez que les noms de gestionnaires fournis dans *Index.cshtml* correspondent aux méthodes de gestionnaire `DeleteAllMessages` et `DeleteMessageAsync` :

[!code-cshtml[Main](razor-pages-convention-features/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` dans le nom de méthode de gestionnaire `DeleteMessageAsync` est exclu par `TryParseHandlerMethod` en raison de la correspondance du gestionnaire de la requête POST à la méthode. Le nom `asp-page-handler` de `DeleteMessage` est mis en correspondance avec la méthode de gestionnaire `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtres MVC et filtre de page (IPageFilter)

Les [filtres d’action](xref:mvc/controllers/filters#action-filters) MVC sont ignorés par les pages Razor, car les pages Razor utilisent des méthodes de gestionnaire. Vous pouvez utiliser d’autres types de filtre MVC : [Autorisation](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Ressource](xref:mvc/controllers/filters#resource-filters) et [Résultat](xref:mvc/controllers/filters#result-filters). Pour plus d’informations, consultez la rubrique [Filtres](xref:mvc/controllers/filters).

Le filtre de page ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) est un filtre qui s’applique aux pages Razor. Il accompagne l’exécution d’une méthode de gestionnaire de page. Il vous permet de traiter du code personnalisé à certaines étapes de l’exécution d’une méthode de gestionnaire de page. Voici un exemple provenant de l’exemple d’application :

[!code-csharp[Main](razor-pages-convention-features/sample/Filters/ReplaceRouteValueFilterAttribute.cs?name=snippet1)]

Ce filtre recherche la valeur de routage « TriggerValue » pour `globalTemplate` et la remplace par « ReplacementValue ».

L’attribut `ReplaceRouteValueFilter` peut être appliqué directement à `PageModel` :

[!code-csharp[Main](razor-pages-convention-features/sample/Pages/OtherPages/Page3.cshtml.cs?range=10-12&highlight=1)]

Demandez la page Page3 de l’exemple d’application sur `localhost:5000/OtherPages/Page3/TriggerValue`. Notez la façon dont le filtre remplace la valeur de routage :

![Si OtherPages/Page3 est demandé avec un segment de routage TriggerValue, le filtre remplace la valeur de routage par ReplacementValue.](razor-pages-convention-features/_static/otherpages-page3-filter-replacement-value.png)

## <a name="see-also"></a>Voir aussi

* [Conventions d’autorisation des pages Razor](xref:security/authorization/razor-pages-authorization)
