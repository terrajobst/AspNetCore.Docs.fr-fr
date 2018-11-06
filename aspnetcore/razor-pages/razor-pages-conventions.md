---
title: Conventions de routes et d’applications pour les pages Razor dans ASP.NET Core
author: guardrex
description: Découvrez comment les conventions du fournisseur de modèles de routes et d’applications vous permettent de gérer le routage, la découverte et le traitement des pages.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/12/2018
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: f04e0930966c9aaf38543729565b1ef4a80a09e2
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207691"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a>Conventions de routes et d’applications pour les pages Razor dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Découvrez comment utiliser les [conventions du fournisseur de modèles de routes et d’applications](xref:mvc/controllers/application-model#conventions) pour contrôler le routage, la découverte et le traitement des pages dans les applications à base de pages Razor.

Quand vous devez configurer des routages de pages personnalisés pour des pages individuelles, configurez le routage vers les pages avec la convention [AddPageRoute](#configure-a-page-route) décrite plus loin dans cette rubrique.

Pour spécifier un itinéraire de page, ajoutez des segments de routage ou ajouter des paramètres à un itinéraire, utilisez la page `@page` directive. Pour plus d’informations, consultez [les itinéraires personnalisés](xref:razor-pages/index#custom-routes).

Il existe des mots réservés qui ne peut pas être utilisés en tant que segments de routage ou des noms de paramètres. Pour plus d’informations, consultez [routage : routage des noms réservés](xref:fundamentals/routing#reserved-routing-names).

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

::: moniker range="= aspnetcore-2.0"

| Scénario | L’exemple montre... |
| -------- | --------------------------- |
| [Conventions de modèle](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li></ul> | Ajouter un modèle de route et un en-tête aux pages d’une application. |
| [Conventions d’actions de routage de pages](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Ajouter un modèle de route aux pages d’un dossier et à une page unique. |
| [Conventions d’actions de modèle de page](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe de filtre, expression lambda ou fabrique de filtres)</li></ul> | Ajouter un en-tête dans les pages d’un dossier, ajouter un en-tête dans une page unique et configurer une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) pour ajouter un en-tête dans les pages d’une application. |
| [Fournisseur de modèles d’applications de pages par défaut](#replace-the-default-page-app-model-provider) | Remplacer le fournisseur de modèles de pages par défaut pour changer les conventions pour les noms du gestionnaire. |

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

| Scénario | L’exemple montre... |
| -------- | --------------------------- |
| [Conventions de modèle](#model-conventions)<br><br>Conventions.Add<ul><li>IPageRouteModelConvention</li><li>IPageApplicationModelConvention</li><li>IPageHandlerModelConvention</li></ul> | Ajouter un modèle de route et un en-tête aux pages d’une application. |
| [Conventions d’actions de routage de pages](#page-route-action-conventions)<ul><li>AddFolderRouteModelConvention</li><li>AddPageRouteModelConvention</li><li>AddPageRoute</li></ul> | Ajouter un modèle de route aux pages d’un dossier et à une page unique. |
| [Conventions d’actions de modèle de page](#page-model-action-conventions)<ul><li>AddFolderApplicationModelConvention</li><li>AddPageApplicationModelConvention</li><li>ConfigureFilter (classe de filtre, expression lambda ou fabrique de filtres)</li></ul> | Ajouter un en-tête dans les pages d’un dossier, ajouter un en-tête dans une page unique et configurer une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) pour ajouter un en-tête dans les pages d’une application. |
| [Fournisseur de modèles d’applications de pages par défaut](#replace-the-default-page-app-model-provider) | Remplacer le fournisseur de modèles de pages par défaut pour changer les conventions pour les noms du gestionnaire. |

::: moniker-end

Les conventions des pages Razor sont configurées à l’aide de la méthode d’extension [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) et ajoutées à [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc) sur la collection de services dans la classe `Startup`. Les exemples de convention suivants sont expliqués plus loin dans cette rubrique :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a>Ordre de l’itinéraire

Itinéraires spécifient un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> pour le traitement (itinéraire correspondant).

| Trier            | Comportement |
| :--------------: | -------- |
| -1               | L’itinéraire est traité avant le traitement des autres itinéraires. |
| 0                | Ordre n’est pas spécifié (valeur par défaut). Ne pas attribuer `Order` (`Order = null`) par défaut est l’itinéraire `Order` à 0 (zéro) pour le traitement. |
| 1, 2, &hellip; n | Spécifie l’ordre de traitement d’itinéraire. |

Traitement de l’itinéraire est établi par convention :

* Les itinéraires sont traités dans un ordre séquentiel (-1, 0, 1, 2, &hellip; n).
* Lorsque les itinéraires ont les mêmes `Order`, le meilleur itinéraire spécifique est mis en correspondance en fonction des itinéraires moins spécifiques.
* Lorsque les itinéraires avec le même `Order` et le même nombre de paramètres correspond à une URL de demande, les itinéraires sont traités dans l’ordre dans lequel elles sont ajoutées à la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.

Si possible, évitez selon un ordre de traitement itinéraire établi. En règle générale, le routage sélectionne la route correcte avec correspondance d’URL. Si vous devez définir des itinéraires `Order` propriétés pour acheminer les demandes correctement, schéma de routage de l’application est probablement à confusion pour les clients et fragiles pour mettre à jour. Pour simplifier le schéma de routage de l’application de recherche. L’exemple d’application nécessite un itinéraire explicite afin de montrer plusieurs scénarios de routage à l’aide d’une seule application de traitement. Toutefois, vous devez essayer d’éviter la pratique de l’itinéraire du paramètre `Order` dans les applications de production.

Le routage de Razor Pages et celui du contrôleur MVC partagent une implémentation. Informations sur l’ordre de routage dans les rubriques MVC sont disponibles à l’adresse [routage vers les actions de contrôleur : classement des routes d’attribut](xref:mvc/controllers/routing#ordering-attribute-routes).

## <a name="model-conventions"></a>Conventions de modèle

Ajoutez un délégué pour [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) afin d’ajouter des [conventions de modèles](xref:mvc/controllers/application-model#conventions) qui s’appliquent aux pages Razor.

### <a name="add-a-route-model-convention-to-all-pages"></a>Ajouter une convention de modèle d’itinéraire à toutes les pages

Utilisez [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) à la collection des instances de [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) qui sont appliquées durant la construction d’un modèle de route de page.

L’exemple d’application ajoute un modèle de routage `{globalTemplate?}` à toutes les pages de l’application :

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `1`. Cela garantit l’itinéraire suivant comportement dans l’exemple d’application de correspondance :

* Un modèle d’itinéraire pour `TheContactPage/{text?}` est ajouté ultérieurement dans cette rubrique. L’itinéraire de la Page Contact a un ordre par défaut de `null` (`Order = 0`), afin qu’il corresponde à avant le `{globalTemplate?}` modèle d’itinéraire.
* Un `{aboutTemplate?}` modèle d’itinéraire est ajouté ultérieurement dans cette rubrique. Le modèle `{aboutTemplate?}` reçoit l’ordre `Order` avec la valeur `2`. Quand la page About est demandée sur `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["aboutTemplate"]` (`Order = 2`) en raison de la définition de la propriété `Order`.
* Un `{otherPagesTemplate?}` modèle d’itinéraire est ajouté ultérieurement dans cette rubrique. Le modèle `{otherPagesTemplate?}` reçoit l’ordre `Order` avec la valeur `2`. Quand une page dans le *Pages/OtherPages* est demandée avec un paramètre d’itinéraire (par exemple, `/OtherPages/Page1/RouteDataValue`), « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) en raison du paramètre le `Order` propriété.

Dans la mesure du possible, ne définissez pas la `Order`, ce qui entraîne des `Order = 0`. S’appuient sur le routage pour sélectionner l’itinéraire correct.

Les options des pages Razor, comme l’ajout de [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions), sont ajoutées au moment de l’ajout de MVC à la collection de services dans `Startup.ConfigureServices`. Pour obtenir un exemple, consultez [l’exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/sample/).

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet1)]

Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue` et examinez le résultat :

![La page About est demandée avec un segment de routage pour GlobalRouteValue. La page affichée montre que la valeur des données de routage est capturée dans la méthode OnGet de la page.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a>Ajouter une convention de modèle d’application à toutes les pages

Utilisez [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) à la collection des instances [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) qui sont appliquées durant la construction d’une application basée sur des pages.

Pour illustrer cette convention et bien d’autres, plus loin dans cette rubrique, l’exemple d’application inclut une classe `AddHeaderAttribute`. Le constructeur de classe accepte une chaîne `name` et un tableau de chaînes `values`. Ces valeurs sont utilisées dans sa méthode `OnResultExecuting` pour définir un en-tête de réponse. La classe complète est affichée dans la section [Conventions d’actions de modèle de page](#page-model-action-conventions), plus loin dans cette rubrique.

L’exemple d’application utilise la classe `AddHeaderAttribute` pour ajouter un en-tête, `GlobalHeader`, à toutes les pages de l’application :

[!code-csharp[](razor-pages-conventions/sample/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

*Startup.cs* :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet2)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que GlobalHeader a été ajouté.](razor-pages-conventions/_static/about-page-global-header.png)

::: moniker range=">= aspnetcore-2.1"

### <a name="add-a-handler-model-convention-to-all-pages"></a>Ajouter une convention de modèle de gestionnaire à toutes les pages

Utilisez [Conventions](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.razorpagesoptions.conventions) pour créer et ajouter un [IPageHandlerModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipagehandlermodelconvention) à la collection des instances [IPageConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageconvention) qui sont appliquées durant la construction d’un modèle de gestionnaire de pages.

```csharp
public class GlobalPageHandlerModelConvention
    : IPageHandlerModelConvention
{
    public void Apply(PageHandlerModel model)
    {
        ...
    }
}
```

`Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
        {
            options.Conventions.Add(new GlobalPageHandlerModelConvention());
        });
```

::: moniker-end

## <a name="page-route-action-conventions"></a>Conventions d’actions de routage de pages

Le fournisseur de modèles de routages par défaut, qui dérive de [IPageRouteModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelprovider), appelle des conventions conçues pour fournir des points d’extensibilité permettant de configurer les routages de pages.

### <a name="folder-route-model-convention"></a>Convention de modèle de routage de dossier

Utilisez [AddFolderRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderroutemodelconvention) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) qui appelle une action sur le [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pour toutes les pages situées dans le dossier spécifié.

L’exemple d’application utilise `AddFolderRouteModelConvention` pour ajouter un modèle de routage `{otherPagesTemplate?}` aux pages du dossier *OtherPages* :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet3)]

La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `2`. Cela garantit que le modèle pour `{globalTemplate?}` (définie plus haut dans la rubrique à `1`) est prioritaire pour les données d’itinéraire première position de la valeur lorsqu’une valeur de routage unique est fournie. Si une page dans le *Pages/OtherPages* est demandée avec une valeur de paramètre d’itinéraire (par exemple, `/OtherPages/Page1/RouteDataValue`), « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) en raison du paramètre le `Order` propriété.

Dans la mesure du possible, ne définissez pas la `Order`, ce qui entraîne des `Order = 0`. S’appuient sur le routage pour sélectionner l’itinéraire correct.

Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` et examinez le résultat :

![La page Page1 du dossier OtherPages est demandée avec un segment de routage pour GlobalRouteValue et OtherPagesRouteValue. La page affichée montre que les valeurs des données de routage sont capturées dans la méthode OnGet de la page.](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a>Convention de modèle de routage de page

Utilisez [AddPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageroutemodelconvention) pour créer et ajouter un [IPageRouteModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageroutemodelconvention) qui appelle une action sur le [PageRouteModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageroutemodel) pour la page ayant le nom spécifié.

L’exemple d’application utilise `AddPageRouteModelConvention` pour ajouter un modèle de routage `{aboutTemplate?}` à la page About :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet4)]

La propriété <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> a la valeur `2`. Cela garantit que le modèle pour `{globalTemplate?}` (définie plus haut dans la rubrique à `1`) est prioritaire pour les données d’itinéraire première position de la valeur lorsqu’une valeur de routage unique est fournie. Si la page About est demandée avec une valeur de paramètre d’itinéraire à `/About/RouteDataValue`, « RouteDataValue » est chargé dans `RouteData.Values["globalTemplate"]` (`Order = 1`) et non `RouteData.Values["aboutTemplate"]` (`Order = 2`) en raison du paramètre le `Order` propriété.

Dans la mesure du possible, ne définissez pas la `Order`, ce qui entraîne des `Order = 0`. S’appuient sur le routage pour sélectionner l’itinéraire correct.

Demandez la page About de l’exemple sur `localhost:5000/About/GlobalRouteValue/AboutRouteValue` et examinez le résultat :

![La page About est demandée avec des segments de routage pour GlobalRouteValue et AboutRouteValue. La page affichée montre que les valeurs des données de routage sont capturées dans la méthode OnGet de la page.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a>Utiliser un transformateur de paramètre pour personnaliser les routages de pages

Les routages de pages générées par ASP.NET Core peuvent être personnalisés à l’aide d’un transformateur de paramètre. Implémente un transformateur de paramètre `IOutboundParameterTransformer` et transforme la valeur des paramètres. Par exemple, un personnalisé `SlugifyParameterTransformer` modifications transformateur de paramètre le `SubscriptionManagement` acheminer la valeur à `subscription-management`.

Le `PageRouteTransformerConvention` convention de modèle de routage de page s’applique un transformateur de paramètre pour les segments de nom de dossier et fichier d’itinéraires de page généré automatiquement dans une application. Par exemple, les Pages Razor fichier */Pages/SubscriptionManagement/ViewAll.cshtml* aurait son itinéraire réécrite depuis `/SubscriptionManagement/ViewAll` à `/subscription-management/view-all`.

`PageRouteTransformerConvention` transforme uniquement les segments générés automatiquement d’un itinéraire de page qui sont fournis à partir du dossier de Pages Razor et le nom de fichier. Il ne transforme pas les segments de routage ajoutés avec la `@page` directive. La convention ne transforme les itinéraires ajoutés par <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.

Le `PageRouteTransformerConvention` est inscrit en tant qu’option dans `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add(
                    new PageRouteTransformerConvention(
                        new SlugifyParameterTransformer()));
            });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

## <a name="configure-a-page-route"></a>Configurer un routage de page

Utilisez [AddPageRoute](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.addpageroute) pour configurer un routage vers une page située dans le chemin spécifié. Les liens générés qui pointent vers la page utilisent le routage que vous avez spécifié. `AddPageRoute` utilise `AddPageRouteModelConvention` pour établir le routage.

L’exemple d’application crée un routage vers `/TheContactPage` pour *Contact.cshtml* :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet5)]

La page Contact est également accessible sur `/Contact` via son routage par défaut.

Le routage personnalisé de l’exemple d’application vers la page Contact permet un segment de routage `text` facultatif (`{text?}`). Cette page inclut également ce segment facultatif dans sa directive `@page`, au cas où le visiteur accèderait à la page via le routage `/Contact` :

[!code-cshtml[](razor-pages-conventions/sample/Pages/Contact.cshtml?highlight=1)]

Notez que l’URL générée pour le lien **Contact** dans la page affichée reflète le routage mis à jour :

![Lien Contact de l’exemple d’application dans la barre de navigation](razor-pages-conventions/_static/contact-link.png)

![Le lien Contact dans la page HTML affichée indique que l’attribut href a la valeur ’/TheContactPage’](razor-pages-conventions/_static/contact-link-source.png)

Visitez la page Contact via son routage ordinaire, `/Contact`, ou via le routage personnalisé, `/TheContactPage`. Si vous indiquez un segment de routage `text` supplémentaire, la page affiche le segment HTML que vous fournissez :

![Exemple dans le navigateur Edge d’un segment de routage ‘text’ facultatif fourni pour ‘TextValue’ dans l’URL. La page affichée montre la valeur du segment ‘text’.](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a>Conventions d’actions de modèle de page

Le fournisseur de modèles de pages par défaut, qui implémente [IPageApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelprovider), appelle des conventions conçues pour fournir des points d’extensibilité permettant de configurer les modèles de pages. Ces conventions sont utiles durant la génération et la modification de scénarios de découverte et de traitement de pages.

Pour les exemples de cette section, l’exemple d’application utilise une classe `AddHeaderAttribute`, qui est un [ResultFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.resultfilterattribute), et qui applique un en-tête de réponse :

[!code-csharp[](razor-pages-conventions/sample/Filters/AddHeader.cs?name=snippet1)]

À l’aide de conventions, l’exemple montre comment appliquer l’attribut à toutes les pages d’un dossier et à une seule page.

**Convention de modèle d’application de dossier**

Utilisez [AddFolderApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addfolderapplicationmodelconvention) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) qui appelle une action sur les instances de [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) pour toutes les pages situées dans le dossier spécifié.

L’exemple illustre l’utilisation de `AddFolderApplicationModelConvention` en ajoutant un en-tête, `OtherPagesHeader`, aux pages situées dans le dossier *OtherPages* de l’application :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet6)]

Demandez la page Page1 de l’exemple sur `localhost:5000/OtherPages/Page1` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page OtherPages/Page1 montrent que OtherPagesHeader a été ajouté.](razor-pages-conventions/_static/page1-otherpages-header.png)

**Convention de modèle d’application de page**

Utilisez [AddPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection.addpageapplicationmodelconvention) pour créer et ajouter un [IPageApplicationModelConvention](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.ipageapplicationmodelconvention) qui appelle une action sur le [PageApplicationModel](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageapplicationmodel) pour la page avec le nom spécifié.

L’exemple illustre l’utilisation de `AddPageApplicationModelConvention` en ajoutant un en-tête, `AboutHeader`, à la page About :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet7)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que AboutHeader a été ajouté.](razor-pages-conventions/_static/about-page-about-header.png)

**Configurer un filtre**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter) configure le filtre spécifié à appliquer. Vous pouvez implémenter une classe de filtre. Toutefois, l’exemple d’application montre comment implémenter un filtre dans une expression lambda, laquelle est implémentée en arrière-plan en tant que fabrique qui retourne un filtre :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet8)]

Le modèle d’application de page est utilisé pour vérifier le chemin relatif des segments qui mènent à la page Page2 dans le dossier *OtherPages*. Si la condition est satisfaite, un en-tête est ajouté. Sinon, `EmptyFilter` est appliqué.

`EmptyFilter` est un [filtre d’action](xref:mvc/controllers/filters#action-filters). Dans la mesure où les filtres d’action sont ignorés par les pages Razor, `EmptyFilter` ne fait rien, comme cela est prévu, si le chemin ne contient pas `OtherPages/Page2`.

Demandez la page Page2 de l’exemple sur `localhost:5000/OtherPages/Page2` et examinez les en-têtes pour voir le résultat :

![OtherPagesPage2Header est ajouté à la réponse pour Page2.](razor-pages-conventions/_static/page2-filter-header.png)

**Configurer une fabrique de filtres**

[ConfigureFilter](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.configurefilter?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_ConfigureFilter_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_Func_Microsoft_AspNetCore_Mvc_ApplicationModels_PageApplicationModel_Microsoft_AspNetCore_Mvc_Filters_IFilterMetadata__) configure la fabrique spécifiée pour appliquer des [filtres](xref:mvc/controllers/filters) à toutes les pages Razor.

L’exemple d’application illustre l’utilisation d’une [fabrique de filtres](xref:mvc/controllers/filters#ifilterfactory) en ajoutant un en-tête, `FilterFactoryHeader`, et deux valeurs aux pages de l’application :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet9)]

*AddHeaderWithFactory.cs* :

[!code-csharp[](razor-pages-conventions/sample/Factories/AddHeaderWithFactory.cs?name=snippet1)]

Demandez la page About de l’exemple sur `localhost:5000/About` et examinez les en-têtes pour voir le résultat :

![Les en-têtes de réponse de la page About montrent que deux en-têtes FilterFactoryHeader ont été ajoutés.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

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

[!code-csharp[](razor-pages-conventions/sample/CustomPageApplicationModelProvider.cs?name=snippet1&highlight=1-2,45-46,64-68,78-85,87,92,106)]

Caractéristiques principales de la classe :

* La classe hérite de `DefaultPageApplicationModelProvider`.
* `TryParseHandlerMethod` traite un gestionnaire pour identifier le verbe HTTP (`httpMethod`) et le nom de gestionnaire avec nom (`handlerName`) durant la création de `PageHandlerModel`.
  * Le suffixe `Async` est ignoré, s’il est présent.
  * La casse est utilisée pour analyser le verbe HTTP à partir du nom de la méthode.
  * Quand le nom de la méthode (sans `Async`) correspond au nom du verbe HTTP, il n’existe aucun gestionnaire nommé. `handlerName` a la valeur `null`, et le nom de la méthode est `Get`, `Post`, `Delete`, `Put` ou `Patch`.
  * Quand le nom de la méthode (sans `Async`) est plus long que le nom du verbe HTTP, il existe un gestionnaire nommé. La propriété `handlerName` a la valeur `<method name (less 'Async', if present)>`. Par exemple, « GetMessage » et « GetMessageAsync » génèrent le nom de gestionnaire « GetMessage ».
  * Les verbes DELETE, PUT et PATCH HTTP sont convertis en POST.

Inscrivez `CustomPageApplicationModelProvider` dans la classe `Startup` :

[!code-csharp[](razor-pages-conventions/sample/Startup.cs?name=snippet10)]

Le modèle de page dans *Index.cshtml.cs* illustre la façon dont les conventions de nommage de méthode de gestionnaire ordinaire changent pour les pages de l’application. Le nommage classique à l’aide du préfixe « On » utilisé avec les pages Razor est supprimé. La méthode qui initialise l’état de la page se nomme désormais `Get`. Vous pouvez constater l’utilisation de cette convention dans l’ensemble de l’application, si vous ouvrez un modèle de page pour l’une des pages.

Chacune des autres méthodes commence par le verbe HTTP qui décrit son traitement. Les deux méthodes qui commencent par `Delete` sont traitées normalement comme des verbes HTTP DELETE, mais la logique dans `TryParseHandlerMethod` définit explicitement le verbe POST pour les deux gestionnaires.

Notez que `Async` est facultatif entre `DeleteAllMessages` et `DeleteMessageAsync`. Les deux méthodes sont asynchrones. Toutefois, vous pouvez choisir d’utiliser ou non le suffixe `Async`. Nous vous recommandons de le faire. `DeleteAllMessages` est utilisé ici à des fins de démonstration, mais nous vous recommandons de nommer cette méthode `DeleteAllMessagesAsync`. Cela n’affecte pas le traitement de l’implémentation de l’exemple. Toutefois, l’utilisation du suffixe `Async` indique qu’il s’agit d’une méthode asynchrone.

[!code-csharp[](razor-pages-conventions/sample/Pages/Index.cshtml.cs?name=snippet1&highlight=1,6,16,29)]

Notez que les noms de gestionnaires fournis dans *Index.cshtml* correspondent aux méthodes de gestionnaire `DeleteAllMessages` et `DeleteMessageAsync` :

[!code-cshtml[](razor-pages-conventions/sample/Pages/Index.cshtml?range=29-60&highlight=7-8,24-25)]

`Async` dans le nom de méthode de gestionnaire `DeleteMessageAsync` est exclu par `TryParseHandlerMethod` en raison de la correspondance du gestionnaire de la requête POST à la méthode. Le nom `asp-page-handler` de `DeleteMessage` est mis en correspondance avec la méthode de gestionnaire `DeleteMessageAsync`.

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a>Filtres MVC et filtre de page (IPageFilter)

Les [filtres d’action](xref:mvc/controllers/filters#action-filters) MVC sont ignorés par les pages Razor, car les pages Razor utilisent des méthodes de gestionnaire. Vous pouvez utiliser d’autres types de filtre MVC : [Autorisation](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Ressource](xref:mvc/controllers/filters#resource-filters) et [Résultat](xref:mvc/controllers/filters#result-filters). Pour plus d’informations, consultez la rubrique [Filtres](xref:mvc/controllers/filters).

Le filtre de page ([IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter)) est un filtre qui s’applique aux pages Razor. Pour plus d’informations, consultez [Méthodes de filtre pour les pages Razor](xref:razor-pages/filter).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Conventions d’autorisation des pages Razor](xref:security/authorization/razor-pages-authorization)
