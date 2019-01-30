---
title: Routage dans ASP.NET Core
author: rick-anderson
description: Découvrez comment le routage ASP.NET Core est responsable du mappage des URI des requêtes aux sélecteurs de point de terminaison et de la distribution des requêtes entrantes entre les points de terminaison.
ms.author: riande
ms.custom: mvc
ms.date: 01/14/2019
uid: fundamentals/routing
ms.openlocfilehash: c5303ad418660fa31fe9094f0e61ee31f5d988f7
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54890014"
---
# <a name="routing-in-aspnet-core"></a>Routage dans ASP.NET Core

Par [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) et [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="<= aspnetcore-1.1"

Pour obtenir la version 1.1 de cette rubrique, téléchargez [Routing in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Le routage est responsable du mappage des URI des requêtes aux sélecteurs de point de terminaison et de la distribution des requêtes entrantes entre les points de terminaison. Les routes sont définies dans l’application et configurées au démarrage de l’application. Une route peut éventuellement extraire des valeurs de l’URL contenue dans la requête, et ces valeurs peuvent ensuite être utilisées pour le traitement de la requête. Avec les informations de routage fournies par l’application, le routage peut également générer des URL qui mappent à des sélecteurs de point de terminaison.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Pour utiliser les scénarios de routage les plus récents dans ASP.NET Core 2.2, spécifiez la [version de compatibilité](xref:mvc/compatibility-version) à l’inscription des services MVC dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

L’option `EnableEndpointRouting` détermine si le routage doit utiliser en interne une logique basée sur les points de terminaison ou la logique basée sur <xref:Microsoft.AspNetCore.Routing.IRouter> d’ASP.NET Core 2.1 ou antérieur. Quand la version de compatibilité est définie sur 2.2 ou ultérieur, la valeur par défaut est `true`. Définissez la valeur sur `false` pour utiliser la logique de routage antérieure :

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Pour plus d’informations sur le routage basé sur <xref:Microsoft.AspNetCore.Routing.IRouter>, consultez la [version ASP.NET Core 2.1 de cette rubrique](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le routage est responsable du mappage des URI des requêtes aux gestionnaires de routage et de la distribution des requêtes entrantes. Les routes sont définies dans l’application et configurées au démarrage de l’application. Une route peut éventuellement extraire des valeurs de l’URL contenue dans la requête, et ces valeurs peuvent ensuite être utilisées pour le traitement de la requête. En utilisant des routes configurées à partir de l’application, le routage est en mesure de générer des URL qui mappent à des gestionnaires de routage.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Pour utiliser les scénarios de routage les plus récents dans ASP.NET Core 2.1, spécifiez la [version de compatibilité](xref:mvc/compatibility-version) à l’inscription des services MVC dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

::: moniker-end

> [!IMPORTANT]
> Ce document traite du routage ASP.NET Core de bas niveau. Pour plus d’informations sur le routage ASP.NET Core MVC, consultez <xref:mvc/controllers/routing>. Pour plus d’informations sur les conventions de routage dans Razor Pages, consultez <xref:razor-pages/razor-pages-conventions>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="routing-basics"></a>Concepts de base du routage

La plupart des applications doivent choisir un schéma de routage de base et descriptif pour que les URL soient lisibles et explicites. La route conventionnelle par défaut `{controller=Home}/{action=Index}/{id?}` :

* Prend en charge un schéma de routage de base et descriptif.
* Est un point de départ pratique pour les applications basées sur une interface utilisateur.

Les développeurs ajoutent fréquemment des routes laconiques supplémentaires aux zones à trafic élevé de l’application dans des situations particulières (par exemple des points de terminaison de blog ou d’e-commerce) en utilisant le [routage d’attributs](xref:mvc/controllers/routing#attribute-routing) ou des routes conventionnelles dédiées.

Les API web doivent utiliser le routage d’attributs pour modéliser les fonctionnalités de l’application sous la forme d’un ensemble de ressources dans lequel les opérations sont représentées par des verbes HTTP. Cela signifie que plusieurs opérations (comme GET et POST) sur la même ressource logique utilisent la même URL. Le routage d’attributs fournit le niveau de contrôle nécessaire pour concevoir avec soin la disposition des points de terminaison publics d’une API.

Les applications Razor Pages utilisent le routage conventionnel par défaut pour délivrer des ressources nommées dans le dossier *Pages* d’une application. Des conventions supplémentaires sont disponibles : elles vous permettent de personnaliser le comportement de routage de Razor Pages. Pour plus d’informations, consultez <xref:razor-pages/index> et <xref:razor-pages/razor-pages-conventions>.

La prise en charge de la génération d’URL permet de développer l’application sans coder en dur les URL pour lier l’application. Cette prise en charge permet de commencer avec une configuration de routage de base, puis de modifier les routes une fois que la disposition des ressources de l’application est déterminée.

::: moniker range=">= aspnetcore-2.2"

Le routage utilise des *points de terminaison* (`Endpoint`) pour représenter les points de terminaison logiques dans une application.

Un point de terminaison définit un délégué pour traiter les requêtes et une collection de métadonnées arbitraires. Les métadonnées sont utilisées pour implémenter des problèmes transversaux basés sur des stratégies et une configuration attachées à chaque point de terminaison.

Le système de routage a les caractéristiques suivantes :

* Une syntaxe des modèles de route est utilisée pour définir des routes avec des paramètres de route tokenisés.
* La configuration de points de terminaison de style conventionnel et de style « attribut » est autorisée.
* `IRouteConstraint` est utilisé pour déterminer si un paramètre d’URL contient une valeur valide pour une contrainte de point de terminaison donné.
* Les modèles d’application, comme MVC/Razor Pages, inscrivent tous leurs points de terminaison, qui ont une implémentation prévisible de scénarios de routage.
* L’implémentation du routage prend des décisions de routage partout où vous le souhaitez dans le pipeline de middleware (intergiciel).
* Le middleware qui apparaît après un middleware de routage peut inspecter le résultat de la décision du middleware de routage quant au point de terminaison d’un URI de requête donné.
* Il est possible d’énumérer tous les points de terminaison dans l’application n’importe où dans le pipeline de middleware.
* Une application peut utiliser le routage pour générer des URL (par exemple pour la redirection ou pour des liens) en fonction des informations des points de terminaison et éviter ainsi les URL codées en dur, ce qui facilite la maintenance.
* La génération d’URL est basée sur des adresses, qui prennent en charge l’extensibilité arbitraire :

  * L’API du générateur de liens (`LinkGenerator`) peut être résolue partout avec [l’injection de dépendances](xref:fundamentals/dependency-injection) pour générer des URL.
  * Quand l’API du générateur de liens n’est pas disponible via l’injection de dépendances, `IUrlHelper` offre des méthodes pour générer des URL.

> [!NOTE]
> Avec la publication du routage des points de terminaison dans ASP.NET Core 2.2, la liaison de point de terminaison est limitée aux actions et aux pages MVC/Razor Pages. Les expansions des fonctionnalités de liaison de point de terminaison sont prévues pour les versions futures.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le routage utilise des *routes* (implémentations de <xref:Microsoft.AspNetCore.Routing.IRouter>) pour :

* Mapper les requêtes entrantes à des *gestionnaires de routage*.
* Générer les URL utilisées dans les réponses.

Par défaut, une application a une seule collection de routes. Quand une requête arrive, les routes de la collection sont traitées dans l’ordre où elles se trouvent dans la collection. Le framework tente de mettre en correspondance l’URL d’une requête entrante avec une route de la collection en appelant la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> sur chaque route de la collection. Une réponse peut utiliser le routage pour générer des URL (par exemple pour la redirection ou pour des liens) en fonction des informations des routes et éviter ainsi les URL codées en dur, ce qui facilite la maintenance.

Le système de routage a les caractéristiques suivantes :

* Une syntaxe des modèles de route est utilisée pour définir des routes avec des paramètres de route tokenisés.
* La configuration de points de terminaison de style conventionnel et de style « attribut » est autorisée.
* `IRouteConstraint` est utilisé pour déterminer si un paramètre d’URL contient une valeur valide pour une contrainte de point de terminaison donné.
* Les modèles d’application, comme MVC/Razor Pages, inscrivent toutes leurs routes, qui ont une implémentation prévisible de scénarios de routage.
* Une réponse peut utiliser le routage pour générer des URL (par exemple pour la redirection ou pour des liens) en fonction des informations des routes et éviter ainsi les URL codées en dur, ce qui facilite la maintenance.
* La génération d’URL est basée sur des routes, qui prennent en charge l’extensibilité arbitraire. `IUrlHelper` offre des méthodes pour générer des URL.

::: moniker-end

Le routage est connecté au pipeline de [l’intergiciel (middleware)](xref:fundamentals/middleware/index) par la classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) ajoute le routage au pipeline de middleware dans le cadre de sa configuration, et gère le routage dans les applications MVC et Razor Pages. Pour découvrir comment utiliser le routage en tant que composant autonome, consultez la section [Utiliser le middleware de routage](#use-routing-middleware).

### <a name="url-matching"></a>Correspondance d’URL

::: moniker range=">= aspnetcore-2.2"

La correspondance d’URL est le processus par lequel le routage distribue une requête entrante à un *point de terminaison*. Ce processus est basé sur des données présentes dans le chemin de l’URL, mais il peut être étendu pour prendre en compte toutes les données de la requête. La possibilité de distribuer des requêtes à des gestionnaires distincts est essentielle pour adapter la taille et la complexité d’une application.

Le système de routage dans le routage de point de terminaison est responsable de toutes les décisions de distribution. Comme le middleware applique des stratégies basées sur le point de terminaison sélectionné, il est important que les décisions susceptibles d’affecter la distribution ou l’application des stratégies de sécurité soient prises au sein du système de routage.

Quand le délégué du point de terminaison est exécuté, les propriétés de `RouteContext.RouteData` sont définies sur des valeurs appropriées en fonction du traitement des requêtes effectué jusqu’à présent.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

La correspondance d’URL est le processus par lequel le routage distribue une requête entrante à un *gestionnaire*. Ce processus est basé sur des données présentes dans le chemin de l’URL, mais il peut être étendu pour prendre en compte toutes les données de la requête. La possibilité de distribuer des requêtes à des gestionnaires distincts est essentielle pour adapter la taille et la complexité d’une application.

Les requêtes entrantes entrent dans `RouterMiddleware`, qui appelle la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> sur chaque route dans l’ordre. L’instance <xref:Microsoft.AspNetCore.Routing.IRouter> détermine s’il faut *gérer* la requête en affectant à [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) un <xref:Microsoft.AspNetCore.Http.RequestDelegate> non null. Si une route définit un gestionnaire pour la requête, le traitement de la route s’arrête et le gestionnaire est appelé pour traiter la requête. Si aucun gestionnaire de routage n’est trouvé pour traiter la requête, le middleware passe la requête au middleware suivant dans le pipeline de requête.

L’entrée principale de `RouteAsync` est le [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associé à la requête actuelle. `RouteContext.Handler` et [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) sont des sorties définies après la mise en correspondance d’une route.

Une correspondance qui appelle `RouteAsync` définit également les propriétés de `RouteContext.RouteData` sur des valeurs appropriées en fonction du traitement des requêtes effectué jusqu’à présent.

::: moniker-end

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) est un dictionnaire de *valeurs de route* produites à partir de la route. Ces valeurs sont généralement déterminées en décomposant l’URL en jetons. Elles peuvent être utilisées pour accepter l’entrée d’utilisateur ou pour prendre d’autres décisions relatives à la distribution à l’intérieur de l’application.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) est un conteneur de propriétés des données supplémentaires associées à la route mise en correspondance. Les `DataTokens` sont fournis pour prendre en charge l’association de données d’état à chaque route, de façon que l’application puisse prendre des décisions en fonction de la route avec laquelle la correspondance a été établie. Ces valeurs sont définies par le développeur et n’affectent **pas** le comportement du routage de quelque manière que ce soit. De plus, les valeurs stockées dans `RouteData.DataTokens` peuvent être de n’importe quel type, contrairement aux `RouteData.Values`, qui doivent être convertibles en chaînes et à partir de chaînes.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) est une liste des routes qui ont participé à la mise en correspondance correcte de la requête. Les routes peuvent être imbriquées les unes dans les autres. La propriété `Routers` reflète le chemin à travers l’arborescence logique des routes qui ont généré une correspondance. En général le premier élément de `Routers` est la collection de routes et il doit être utilisé pour la génération d’URL. Le dernier élément de `Routers` est le gestionnaire de routage avec lequel la correspondance a été établie.

### <a name="url-generation"></a>Génération d’URL

::: moniker range=">= aspnetcore-2.2"

La génération d’URL est le processus par lequel le routage peut créer un chemin d’URL basé sur un ensemble de valeurs de route. Ceci permet une séparation logique entre vos points de terminaison et les URL qui y accèdent.

Le routage des points de terminaison inclut l’API de générateur de liens (`LinkGenerator`). `LinkGenerator` est un service singleton qui peut être récupéré à partir de l’injection de dépendances. L’API peut être utilisée en dehors du contexte d’une requête en cours d’exécution. Le `IUrlHelper` de MVC et les scénarios qui s’appuient sur `IUrlHelper`, comme les [Tag Helpers](xref:mvc/views/tag-helpers/intro), les helpers HTML et les [résultats d’action](xref:mvc/controllers/actions), utilisent le générateur de liens pour fournir les fonctionnalités de création de liens.

Le générateur de liens est basé sur le concept d’une *adresse* et de *schémas d’adresse*. Un schéma d’adresse est un moyen de déterminer les points de terminaison à prendre en compte pour la génération de liens. Par exemple, les scénarios de nom de route et de valeurs de route que connaissent bien nombre d’utilisateurs dans les MVC/Razor Pages sont implémentés en tant que schémas d’adresse.

Le générateur de liens peut lier à des actions et à des pages MVC/Razor Pages via les méthodes d’extension suivantes :

* `GetPathByAction`
* `GetUriByAction`
* `GetPathByPage`
* `GetUriByPage`

Une surcharge de ces méthodes accepte des arguments qui incluent le `HttpContext`. Ces méthodes sont fonctionnellement équivalentes à `Url.Action` et à `Url.Page`, mais elles offrent davantage de flexibilité et d’options.

Les méthodes `GetPath*` sont les plus proches de `Url.Action` et de `Url.Page` en ce qu’elles génèrent un URI contenant un chemin absolu. Les méthodes `GetUri*` génèrent toujours un URI absolu contenant un schéma et un hôte. Les méthodes qui acceptent un `HttpContext` génèrent un URI dans le contexte de la requête en cours d’exécution. Les valeurs de route ambiante, le chemin de base d’URL, le schéma et l’hôte de la requête en cours d’exécution sont utilisés, sauf s’ils sont remplacés.

`LinkGenerator` est appelé avec une adresse. La génération d’un URI se fait en deux étapes :

1. Une adresse est liée à une liste de points de terminaison qui correspondent à l’adresse.
1. Le `RoutePattern` de chaque point de terminaison est évalué jusqu’à ce qu’un modèle de route correspondant aux valeurs fournies soit trouvé. Le résultat obtenu est combiné avec d’autres parties de l’URI fournies par le générateur de liens, puis il est retourné.

Les méthodes fournies par `LinkGenerator` prennent en charge des fonctionnalités de génération de liens standard pour n’importe quel type d’adresse. La façon la plus pratique d’utiliser le générateur de liens est de le faire via des méthodes d’extension qui effectuent des opérations pour un type d’adresse spécifique.

| Méthode d’extension   | Description                                                         |
| ------------------ | ------------------------------------------------------------------- |
| `GetPathByAddress` | Génère un URI avec un chemin absolu basé sur les valeurs fournies. |
| `GetUriByAddress`  | Génère un URI absolu basé sur les valeurs fournies.             |

> [!WARNING]
> Faites attention aux implications suivantes de l’appel de méthodes `LinkGenerator` :
>
> * Utilisez les méthodes d’extension `GetUri*` avec précaution dans une configuration d’application qui ne valide pas l’en-tête `Host` des requêtes entrantes. Si l’en-tête `Host` des requêtes entrantes n’est pas validé, l’entrée de requête non approuvée peut être renvoyée au client dans les URI d’une page/vue. Nous recommandons que toutes les applications de production configurent leur serveur pour qu’il valide l’en-tête `Host` par rapport à des valeurs valides connues.
>
> * Utilisez `LinkGenerator` avec précaution dans le middleware en combinaison avec `Map` ou `MapWhen`. `Map*` modifie le chemin de base de la requête en cours d’exécution, ce qui affecte la sortie de la génération de liens. Toutes les API `LinkGenerator` permettent la spécification d’un chemin de base. Spécifiez toujours un chemin de base vide pour annuler l’effet de `Map*` sur la génération de liens.

## <a name="differences-from-earlier-versions-of-routing"></a>Différences par rapport aux versions précédentes du routage

Il existe quelques différences entre le routage de points de terminaison d’ASP.NET Core 2.2 ou ultérieur et celui des versions antérieures d’ASP.NET Core :

* Le système de routage de points de terminaison ne prend pas en charge l’extensibilité basée sur `IRouter`, notamment l’héritage de `Route`.

* Le routage de points de terminaison ne prend pas en charge [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim). Utilisez la [version de compatibilité](xref:mvc/compatibility-version) 2.1 (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) pour continuer à utiliser le shim de compatibilité.

* Le routage de points de terminaison a un comportement différent pour la casse des URI générés lors de l’utilisation de routes conventionnelles.

  Considérez le modèle de route par défaut suivant :

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supposez que vous générez un lien vers une action avec la route suivante :

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  Avec le routage basé sur `IRouter`, ce code génère un URI `/blog/ReadPost/17`, qui respecte la casse de la valeur de la route fournie. Le routage de points de terminaison dans ASP.NET Core 2.2 ou ultérieur produit `/Blog/ReadPost/17` (« Blog » commence par une majuscule). Le routage de points de terminaison fournit l’interface `IOutboundParameterTransformer`, qui peut être utilisée pour personnaliser ce comportement de façon globale ou pour appliquer des conventions différentes pour le mappage d’URL.

  Pour plus d’informations, consultez la section [Informations de référence sur les transformateurs de paramètre](#parameter-transformer-reference).

* La génération de liens utilisée par MVC/Razor Pages avec des routes conventionnelles se comporte différemment quand vous tentez de lier à un contrôleur/action ou à une page qui n’existe pas.

  Considérez le modèle de route par défaut suivant :

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  Supposez que vous générez un lien vers une action en utilisant le modèle par défaut avec ce qui suit :

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  Avec le routage basé sur `IRouter`, le résultat est toujours `/Blog/ReadPost/17`, même si le `BlogController` n’existe pas ou n’a pas de méthode d’action `ReadPost`. Comme prévu, le routage de points de terminaison dans ASP.NET Core 2.2 ou ultérieur produit `/Blog/ReadPost/17` si la méthode d’action existe. *Cependant, le routage de points de terminaison produit une chaîne vide si l’action n’existe pas.* Sur le plan conceptuel, le routage de points de terminaison ne fait pas l’hypothèse que le point de terminaison existe si l’action n’existe pas.

* L’*algorithme d’invalidation de valeur ambiante* de la génération de liens se comporte différemment quand il est utilisé avec le routage de points de terminaison.

  L’*invalidation de valeur ambiante* est l’algorithme qui décide quelles valeurs de route provenant de la requête en cours d’exécution (les valeurs ambiantes) peuvent être utilisées dans les opérations de génération de liens. Le routage conventionnel a toujours invalidé les valeurs de route supplémentaires lors de la liaison à une action différente. Le routage d’attributs n’a pas ce comportement avant la publication d’ASP.NET Core 2.2. Dans les versions antérieures d’ASP.NET Core, les liens vers une autre action utilisant les mêmes noms de paramètre de route provoquaient des erreurs de génération de liens. Dans ASP.NET Core 2.2 ou ultérieur, les deux formes de routage invalident les valeurs lors de la liaison vers une autre action.

  Considérez l’exemple suivant dans ASP.NET Core 2.1 ou antérieur. Lors de la liaison à une autre action (ou une autre page), les valeurs de route peuvent être réutilisées de façon non souhaitée.

  Dans */Pages/Store/Product.cshtml* :

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  Dans */Pages/Login.cshtml* :

  ```cshtml
  @page "{id?}"
  ```

  Si l’URI est `/Store/Product/18` dans ASP.NET Core 2.1 ou antérieur, le lien généré sur la page Store/Info par `@Url.Page("/Login")` est `/Login/18`. La valeur 18 pour `id` est réutilisée, même si la destination du lien est une partie entièrement différente de l’application. La valeur de route pour `id` dans le contexte de la page `/Login` est probablement une valeur d’ID utilisateur, et non pas une valeur d’ID de produit de magasin.

  Dans le routage de points de terminaison avec ASP.NET Core 2.2 ou ultérieur, le résultat est `/Login`. Les valeurs ambiantes ne sont pas réutilisées quand la destination liée est une action ou une page différente.

* Syntaxe des paramètres de route avec aller-retour : les barres obliques ne sont pas encodées lors de l’utilisation d’une syntaxe de paramètre passe-partout avec double astérisque (`**`).

  Pendant la génération de liens, le système de routage encode la valeur capturée dans un paramètre passe-partout avec double astérisque (`**`) (par exemple `{**myparametername}`) sans les barres obliques. Le passe-partout avec double astérisque est pris en charge avec le routage basé sur `IRouter` dans ASP.NET Core 2.2 ou ultérieur.

  La syntaxe de paramètre passe-partout avec un seul astérisque dans les versions antérieures d’ASP.NET Core (`{*myparametername}`) reste prise en charge, et les barres obliques sont encodées.

  | Route              | Lien généré avec<br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | `/search/admin%2Fproducts` (la barre oblique est encodée)             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a>Exemple de middleware

Dans l’exemple suivant, un middleware utilise l’API `LinkGenerator` pour créer un lien vers une méthode d’action qui liste les produits d’un magasin. L’utilisation du générateur de liens en l’injectant dans une classe et en appelant `GenerateLink` est disponible pour n’importe quelle classe dans une application.

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

La génération d’URL est le processus par lequel le routage peut créer un chemin d’URL basé sur un ensemble de valeurs de route. Ceci permet une séparation logique entre les gestionnaires de routes et les URL qui y accèdent.

La génération d’URL suit un processus itératif similaire, mais elle commence par un appel du code utilisateur ou de framework à la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> de la collection de routes. La méthode `GetVirtualPath` de chaque *route* est appelée en séquence jusqu’à ce qu’un <xref:Microsoft.AspNetCore.Routing.VirtualPathData> non null soit retourné.

Les entrées principales dans `GetVirtualPath` sont les suivantes :

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Les routes utilisent principalement les valeurs de routage fournies par `Values` et par `AmbientValues` pour décider où il est possible de générer une URL et quelles sont les valeurs à inclure. Les `AmbientValues` sont l’ensemble des valeurs de route produites à partir de la mise en correspondance de la requête actuelle. En revanche, les `Values` sont les valeurs de route qui spécifient la façon de générer l’URL souhaitée pour l’opération actuelle. `HttpContext` est fourni au cas où une route aurait besoin d’obtenir des services ou des données supplémentaires associés au contexte actuel.

> [!TIP]
> Considérez [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) comme un ensemble de remplacements pour [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). La génération d’URL tente de réutiliser des valeurs de route de la requête actuelle afin de générer les URL pour des liens utilisant la même route ou les mêmes valeurs de route.

La sortie de `GetVirtualPath` est un `VirtualPathData`. `VirtualPathData` est l’équivalent de `RouteData`. `VirtualPathData` contient le `VirtualPath` de l’URL de sortie et des propriétés supplémentaires qui doivent être définies par la route.

La propriété [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contient le *chemin virtuel* produit par la route. Selon vos besoins, vous devrez peut-être traiter plus avant le chemin. Si vous souhaitez afficher l’URL générée au format HTML, ajoutez un préfixe au chemin de base de l’application.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) est une référence à la route qui a généré avec succès l’URL.

La propriété [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) est un dictionnaire de données supplémentaires relatives à la route qui a généré l’URL. Il s’agit de l’équivalent de [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

::: moniker-end

### <a name="create-routes"></a>Créer des routes

::: moniker range="< aspnetcore-2.2"

Le routage fournit la classe <xref:Microsoft.AspNetCore.Routing.Route> comme implémentation standard d’<xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` utilise la syntaxe des *modèles de routage* pour définir des modèles à mettre en correspondance avec le chemin d’URL quand <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> est appelé. `Route` utilise le même modèle de routage pour générer une URL quand `GetVirtualPath` est appelé.

::: moniker-end

La plupart des applications créent des routes en appelant <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> ou l’une des méthodes d’extension similaires définies sur <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Toutes les méthodes d’extension de `IRouteBuilder` créent une instance de <xref:Microsoft.AspNetCore.Routing.Route> et l’ajoutent à la collection de routes.

::: moniker range=">= aspnetcore-2.2"

`MapRoute` n’accepte pas de paramètre de gestionnaire de routage. `MapRoute` ajoute uniquement des routes gérées par <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Pour plus d’informations sur le routage dans MVC, consultez <xref:mvc/controllers/routing>.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

`MapRoute` n’accepte pas de paramètre de gestionnaire de routage. `MapRoute` ajoute uniquement des routes gérées par <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Le gestionnaire par défaut est un `IRouter`, et le gestionnaire est susceptible de ne pas traiter la requête. Par exemple, ASP.NET Core MVC est généralement configuré comme gestionnaire par défaut qui gère uniquement les requêtes correspondant à un contrôleur et une action disponibles. Pour plus d’informations sur le routage dans MVC, consultez <xref:mvc/controllers/routing>.

::: moniker-end

L’exemple de code suivant est un exemple d’appel à `MapRoute` utilisé par une définition de route ASP.NET Core MVC classique :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ce modèle établit une correspondance avec un chemin d’URL et extrait les valeurs de route . Par exemple, le chemin `/Products/Details/17` génère les valeurs de route suivantes : `{ controller = Products, action = Details, id = 17 }`.

Les valeurs de route sont déterminées en divisant le chemin d’URL en segments et en mettant en correspondance chaque segment avec le nom des *paramètres de routage* dans le modèle de routage. Les paramètres de routage sont nommés. Vous définissez des paramètres en plaçant leur nom entre des accolades `{ ... }`.

Le modèle précédent peut également mettre en correspondance le chemin d’URL `/` et produire les valeurs `{ controller = Home, action = Index }`. Cela s’explique par le fait que les paramètres de routage `{controller}` et `{action}` ont des valeurs par défaut et que le paramètre de routage `id` est facultatif. Un signe égal (`=`) suivi d’une valeur après le nom du paramètre de routage définit une valeur par défaut pour le paramètre. Un point d’interrogation (`?`) après le nom du paramètre de routage définit un paramètre facultatif.

Les paramètres de routage ayant une valeur par défaut produisent *toujours* une valeur de routage quand la route correspond. Les paramètres facultatifs ne produisent pas de valeur de routage si aucun segment de chemin d’URL ne correspondait. Pour obtenir une description complète des scénarios et de la syntaxe des modèles de routage, consultez la section [Informations de référence sur les modèles de routage](#route-template-reference).

Dans l’exemple suivant, la définition du paramètre de route `{id:int}` définit une [contrainte de route](#route-constraint-reference) pour le paramètre de route `id` :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ce modèle établit une correspondance avec un chemin d’URL comme `/Products/Details/17`, mais pas `/Products/Details/Apples`. Les contraintes de routage implémentent <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> et inspectent les valeurs de route pour les vérifier. Dans cet exemple, la valeur de route `id` doit être convertible en entier. Pour obtenir une explication des contraintes de route fournies par le framework, consultez [Informations de référence sur les contraintes de route](#route-constraint-reference).

Des surcharges supplémentaires de `MapRoute` acceptent des values pour `constraints`, `dataTokens` et `defaults`. L’utilisation classique de ces paramètres consiste à passer un objet typé anonymement, où les noms des propriétés du type anonyme correspondent aux noms de paramètre de routage.

Les exemples de `MapRoute` suivants créent des routes équivalentes :

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> La syntaxe inline pour la définition des contraintes et des valeurs par défaut peut être pratique pour les routes simples. Cependant, certains scénarios, comme les jetons de données, ne sont pas pris en charge par la syntaxe inline.

L’exemple suivant montre quelques autres scénarios :

::: moniker range=">= aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Le modèle précédent établit une correspondance avec un chemin d’URL comme `/Blog/All-About-Routing/Introduction` et extrait les valeurs `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Les valeurs de route par défaut pour `controller` et `action` sont produites par la route, même s’il n’existe aucun paramètre de routage correspondant dans le modèle. Il est possible de spécifier des valeurs par défaut dans le modèle de route. Le paramètre de route `article` est défini comme *passe-partout*  par la présence d’un double astérisque (`**`) avant le nom du paramètre de route. Les paramètres de routage fourre-tout capturent le reste du chemin d’URL et peuvent également établir une correspondance avec la chaîne vide.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Le modèle précédent établit une correspondance avec un chemin d’URL comme `/Blog/All-About-Routing/Introduction` et extrait les valeurs `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Les valeurs de route par défaut pour `controller` et `action` sont produites par la route, même s’il n’existe aucun paramètre de routage correspondant dans le modèle. Il est possible de spécifier des valeurs par défaut dans le modèle de route. Le paramètre de route `article` est défini comme *passe-partout*  par la présence d’un astérisque (`*`) avant le nom du paramètre de route. Les paramètres de routage fourre-tout capturent le reste du chemin d’URL et peuvent également établir une correspondance avec la chaîne vide.

::: moniker-end

L’exemple suivant ajoute des contraintes de route et des jetons de données :

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Le modèle précédent établit une correspondance avec un chemin d’URL comme `/en-US/Products/5`, et extrait les valeurs `{ controller = Products, action = Details, id = 5 }` et les jetons de données `{ locale = en-US }`.

![Jetons Windows de variables locales](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a>Génération d’URL de classe de route

La classe `Route` peut également effectuer une génération d’URL en combinant un ensemble de valeurs de route et son modèle de routage. Il s’agit logiquement du processus inverse de la mise en correspondance du chemin d’URL.

> [!TIP]
> Pour mieux comprendre la génération d’URL, imaginez l’URL que vous voulez générer, puis pensez à la façon dont un modèle de routage établirait une correspondance avec cette URL. Quelles valeurs seraient produites ? Cela équivaut approximativement à la façon dont la génération d’URL fonctionne dans la classe `Route`.

L’exemple suivant utilise une route par défaut ASP.NET Core MVC générale :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Avec les valeurs de route `{ controller = Products, action = List }`, l’URL `/Products/List` est générée. Les valeurs de route remplacent les paramètres de routage correspondant pour former le chemin d’URL. Dans la mesure où `id` est un paramètre de route facultatif, l’URL est générée correctement sans valeur pour `id`.

Avec les valeurs de route `{ controller = Home, action = Index }`, l’URL `/` est générée. Les valeurs de route fournies correspondent aux valeurs par défaut, et les segments correspondant aux valeurs par défaut peuvent être omis sans risque.

Les deux URL générées effectuent un aller-retour avec la définition de route suivante (`/Home/Index` et `/`), et produisent les mêmes valeurs de route que celles utilisées pour générer l’URL.

> [!NOTE]
> Pour générer des URL, une application utilisant ASP.NET Core MVC doit utiliser <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> au lieu d’effectuer un appel directement dans le routage.

Pour plus d’informations sur la génération d’URL, consultez la section [Informations de référence sur la génération d’URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Utilisation du middleware de routage

Référencez le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) dans le fichier projet de l’application.

Ajoutez le routage au conteneur de service dans `Startup.ConfigureServices` :

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Les routes doivent être configurées dans la méthode `Startup.Configure`. L’exemple d’application utilise les API suivantes :

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Établit une correspondance uniquement avec les requêtes HTTP GET.
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

Le tableau suivant montre les réponses avec les URI donnés.

| URI                    | Réponse                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | Hello! Valeurs de route : [operation, create], [id, 3] |
| `/package/track/-3`    | Hello! Valeurs de route : [operation, track], [id, -3] |
| `/package/track/-3/`   | Hello! Valeurs de route : [operation, track], [id, -3] |
| `/package/track/`      | La requête passe à travers ceci, aucune correspondance.              |
| `GET /hello/Joe`       | Hi, Joe!                                          |
| `POST /hello/Joe`      | La requête passe à travers ceci, correspondance seulement avec HTTP GET. |
| `GET /hello/Joe/Smith` | La requête passe à travers ceci, aucune correspondance.              |

::: moniker range="< aspnetcore-2.2"

Si vous configurez une seule route, appelez <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> en passant une instance `IRouter`. Vous n’avez pas besoin d’utiliser <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

::: moniker-end

Le framework fournit un ensemble de méthodes d’extension pour la création de routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>) :

* `MapDelete`
* `MapGet`
* `MapMiddlewareDelete`
* `MapMiddlewareGet`
* `MapMiddlewarePost`
* `MapMiddlewarePut`
* `MapMiddlewareRoute`
* `MapMiddlewareVerb`
* `MapPost`
* `MapPut`
* `MapRoute`
* `MapVerb`

::: moniker range="< aspnetcore-2.2"

Certaines des méthodes listées, comme `MapGet`, nécessitent un `RequestDelegate`. `RequestDelegate` est utilisé comme *gestionnaire de routage* quand une correspondance est trouvée pour la route. D’autres méthodes de cette famille permettent de configurer un pipeline de middleware utilisé comme gestionnaire de routage. Si la méthode `Map*` n’accepte pas de gestionnaire, comme `MapRoute`, elle utilise le <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

::: moniker-end

Les méthodes `Map[Verb]` utilisent des contraintes pour limiter la route au verbe HTTP dans le nom de la méthode. Par exemple, consultez <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> et <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Informations de référence sur les modèles de routage

Les jetons placés entre accolades (`{ ... }`) définissent des *paramètres de routage* qui sont liés si une correspondance est trouvée pour la route. Vous pouvez définir plusieurs paramètres de routage dans un segment de route, mais ils doivent être séparés par une valeur littérale. Par exemple `{controller=Home}{action=Index}` n’est pas une route valide, car il n’y a aucune valeur littérale entre `{controller}` et `{action}`. Ces paramètres de routage doivent avoir un nom, et ils autorisent la spécification d’attributs supplémentaires.

Un texte littéral autre que les paramètres de routage (par exemple, `{id}`) et le séparateur de chemin `/` doit correspondre au texte présent dans l’URL. La correspondance de texte ne respecte pas la casse et est basée sur la représentation décodée du chemin des URL. Pour mettre en correspondance un délimiteur de paramètre de route littéral (`{` ou `}`), placez-le dans une séquence d’échappement en répétant le caractère (`{{` ou `}}`).

Les modèles d’URL qui tentent de capturer un nom de fichier avec une extension de fichier facultative doivent faire l’objet de considérations supplémentaires. Prenez par exemple le modèle `files/{filename}.{ext?}`. Quand des valeurs existent à la fois pour `filename` et pour `ext`, les deux valeurs sont renseignées. Si seule une valeur existe pour `filename` dans l’URL, une correspondance est trouvée pour la route, car le point final (`.`) est facultatif. Les URL suivantes correspondent à cette route :

* `/files/myFile.txt`
* `/files/myFile`

::: moniker range=">= aspnetcore-2.2"

Vous pouvez utiliser un astérisque (`*`) ou un double astérisque (`**`) comme préfixe d’un paramètre de route à lier au reste de l’URI. Ils sont appelés des paramètres *passe-partout*. Par exemple, `blog/{**slug}` établit une correspondance avec n’importe quel URI commençant par `/blog` et suivi de n’importe quelle valeur, qui est affectée à la valeur de route `slug`. Les paramètres fourre-tout peuvent également établir une correspondance avec la chaîne vide.

Le paramètre fourre-tout place les caractères appropriés dans une séquence d’échappement lorsque la route est utilisée pour générer une URL, y compris les caractères de séparation de chemin (`/`). Par exemple, la route `foo/{*path}` avec les valeurs de route `{ path = "my/path" }` génère `foo/my%2Fpath`. Notez la barre oblique d’échappement. Pour les séparateurs de chemin aller-retour, utilisez le préfixe de paramètre de routage `**`. La route `foo/{**path}` avec `{ path = "my/path" }` génère `foo/my/path`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Vous pouvez utiliser l’astérisque (`*`) comme préfixe d’un paramètre de route à lier au reste de l’URI. Cela s’appelle un paramètre *fourre-tout*. Par exemple, `blog/{*slug}` établit une correspondance avec n’importe quel URI commençant par `/blog` et suivi de n’importe quelle valeur, qui est affectée à la valeur de route `slug`. Les paramètres fourre-tout peuvent également établir une correspondance avec la chaîne vide.

Le paramètre fourre-tout place les caractères appropriés dans une séquence d’échappement lorsque la route est utilisée pour générer une URL, y compris les caractères de séparation de chemin (`/`). Par exemple, la route `foo/{*path}` avec les valeurs de route `{ path = "my/path" }` génère `foo/my%2Fpath`. Notez la barre oblique d’échappement.

::: moniker-end

Les paramètres de route peuvent avoir des *valeurs par défaut*, désignées en spécifiant la valeur par défaut après le nom du paramètre, séparée par un signe égal (`=`). Par exemple, `{controller=Home}` définit `Home` comme valeur par défaut de `controller`. La valeur par défaut est utilisée si aucune valeur n’est présente dans l’URL pour le paramètre. Vous pouvez rendre facultatifs les paramètres de route en ajoutant un point d’interrogation (`?`) à la fin du nom du paramètre, comme dans `id?`. La différence entre les valeurs facultatives et les paramètres de route par défaut est qu’un paramètre de route ayant une valeur par défaut produit toujours une valeur, tandis qu’un paramètre facultatif a une valeur seulement quand celle-ci est fournie par l’URL de requête.

Les paramètres de route peuvent avoir des contraintes, qui doivent correspondre à la valeur de route liée à partir de l’URL. L’ajout d’un signe deux-points (`:`) et d’un nom de contrainte après le nom du paramètre de routage spécifie une *contrainte inline* sur un paramètre de routage. Si la contrainte nécessite des arguments, ils sont fournis entre parenthèses (`(...)`) après le nom de la contrainte. Il est possible de spécifier plusieurs contraintes inline en ajoutant un autre signe deux-points (`:`) et le nom d’une autre contrainte.

Le nom de la contrainte et les arguments sont passés au service <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> pour créer une instance de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> à utiliser dans le traitement des URL. Par exemple, le modèle de routage `blog/{article:minlength(10)}` spécifie une contrainte `minlength` avec l’argument `10`. Pour plus d’informations sur les contraintes de route et pour obtenir la liste des contraintes fournies par le framework, consultez la section [Informations de référence sur les contraintes de route](#route-constraint-reference).

::: moniker range=">= aspnetcore-2.2"

Les paramètres de route peuvent également contenir des transformateurs de paramètre, qui transforment une valeur de paramètre lors de la génération des liens et de la mise en correspondance des actions et des pages avec les URL. À l’instar des contraintes, les transformateurs de paramètre peuvent être ajoutés inline à un paramètre de routage en ajoutant un signe deux-points (`:`) et le nom du transformateur après le nom du paramètre de routage. Par exemple, le modèle de routage `blog/{article:slugify}` spécifie un transformateur `slugify`. Pour plus d’informations sur les transformateurs de paramètre, consultez la section [Informations de référence sur les transformateurs de paramètre](#parameter-transformer-reference).

::: moniker-end

Le tableau suivant montre des exemples de modèles de route et leur comportement.

| Modèle de routage                           | Exemple d’URI en correspondance    | URI de la requête&hellip;                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | Correspond seulement au chemin unique `/hello`.                                     |
| `{Page=Home}`                            | `/`                     | Correspond à `Page` et le définit sur `Home`.                                         |
| `{Page=Home}`                            | `/Contact`              | Correspond à `Page` et le définit sur `Contact`.                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | Mappe au contrôleur `Products` et à l’action `List`.                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | Mappe au contrôleur `Products` et à l’action `Details` (`id` défini sur 123). |
| `{controller=Home}/{action=Index}/{id?`} | `/`                     | Mappe au contrôleur `Home` et à la méthode `Index` (`id` est ignoré).        |

L’utilisation d’un modèle est généralement l’approche la plus simple pour le routage. Il est également possible de spécifier des contraintes et des valeurs par défaut hors du modèle de routage.

> [!TIP]
> Activez la [journalisation](xref:fundamentals/logging/index) pour voir comment les implémentations de routage intégrées, comme `Route`, établissent des correspondances avec les requêtes.

## <a name="reserved-routing-names"></a>Noms de routage réservés

Les mots clés suivants sont des noms réservés qui ne peuvent pas être utilisés comme paramètres ou noms de routage :

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Informations de référence sur les contraintes de routage

Les contraintes de route s’exécutent quand une correspondance s’est produite pour l’URL entrante, et le chemin de l’URL est tokenisé en valeurs de route. En général, les contraintes de routage inspectent la valeur de route associée par le biais du modèle de routage, et créent une décision oui/non indiquant si la valeur est, ou non, acceptable. Certaines contraintes de routage utilisent des données hors de la valeur de route pour déterminer si la requête peut être routée. Par exemple, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> peut accepter ou rejeter une requête en fonction de son verbe HTTP. Les contraintes sont utilisées dans le routage des requêtes et la génération des liens.

> [!WARNING]
> N’utilisez pas de contraintes pour la **validation des entrées**. Si des contraintes sont utilisées pour la **validation des entrées**, une entrée non valide génère une réponse *404 - Introuvable* au lieu d’une réponse *400 - Requête incorrecte* avec un message d’erreur approprié. Les contraintes de route sont utilisées pour **lever l’ambiguïté** entre des routes similaires, et non pas pour valider les entrées d’une route particulière.

Le tableau suivant montre des exemples de contrainte de route et leur comportement attendu.

| contrainte | Exemple | Exemples de correspondances | Notes |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789` | Correspond à n’importe quel entier |
| `bool` | `{active:bool}` | `true`, `FALSE` | Correspond à `true` ou à `false` (non-respect de la casse) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm` | Correspond à une valeur `DateTime` valide (dans la culture invariante ; voir l’avertissement) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Correspond à une valeur `decimal` valide (dans la culture invariante ; voir l’avertissement) |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Correspond à une valeur `double` valide (dans la culture invariante ; voir l’avertissement) |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Correspond à une valeur `float` valide (dans la culture invariante ; voir l’avertissement) |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Correspond à une valeur `Guid` valide |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Correspond à une valeur `long` valide |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | La chaîne doit comporter au moins 4 caractères |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | La chaîne ne doit pas comporter plus de 8 caractères |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | La chaîne doit comporter exactement 12 caractères |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | La chaîne doit comporter au moins 8 caractères et pas plus de 16 caractères |
| `min(value)` | `{age:min(18)}` | `19` | La valeur entière doit être au moins égale à 18 |
| `max(value)` | `{age:max(120)}` | `91` | La valeur entière ne doit pas être supérieure à 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | La valeur entière doit être au moins égale à 18 mais ne doit pas être supérieure à 120 |
| `alpha` | `{name:alpha}` | `Rick` | La chaîne doit se composer d’un ou de plusieurs caractères alphabétiques (`a`-`z`, non-respect de la casse) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | La chaîne doit correspondre à l’expression régulière (voir les conseils relatifs à la définition d’une expression régulière) |
| `required` | `{name:required}` | `Rick` | Utilisé pour garantir qu’une valeur autre qu’un paramètre est présente pendant la génération de l’URL |

Il est possible d’appliquer plusieurs contraintes séparées par un point-virgule à un même paramètre. Par exemple, la contrainte suivante limite un paramètre à une valeur entière supérieure ou égale à 1 :

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant. ces contraintes partent du principe que l’URL n’est pas localisable. Les contraintes de routage fournies par le framework ne modifient pas les valeurs stockées dans les valeurs de route. Toutes les valeurs de route analysées à partir de l’URL sont stockées sous forme de chaînes. Par exemple, la contrainte `float` tente de convertir la valeur de route en valeur float, mais la valeur convertie est utilisée uniquement pour vérifier qu’elle peut être convertie en valeur float.

## <a name="regular-expressions"></a>Expressions régulières

Le framework ASP.NET Core ajoute `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` au constructeur d’expression régulière. Pour obtenir une description de ces membres, consultez <xref:System.Text.RegularExpressions.RegexOptions>.

Les expressions régulières utilisent les délimiteurs et des jetons semblables à ceux utilisés par le service de routage et le langage C#. Les jetons d’expression régulière doivent être placés dans une séquence d’échappement. Pour que l’expression régulière `^\d{3}-\d{2}-\d{4}$` puisse être utilisée dans le routage, il faut que les caractères `\` (une seule barre oblique inverse) soit fournis dans la chaîne sous la forme `\\` (double barre oblique inverse) dans le fichier source C#, pour placer en échappement le caractère d’échappement de chaîne `\` (sauf en cas d’utilisation de [littéraux de chaîne textuelle](/dotnet/csharp/language-reference/keywords/string)). Pour placer en échappement les caractères de délimiteur de paramètre de route (`{`, `}`, `[`, `]`), doublez les caractères dans l’expression (`{{`, `}`, `[[`, `]]`). Le tableau suivant montre une expression régulière et la version placée en échappement.

| Expression régulière    | Expression régulière en échappement     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Les expressions régulières utilisées dans le routage commencent souvent par un caret (`^`) et correspondent à la position de début de la chaîne. Les expressions se terminent souvent par le signe dollar (`$`) de caractère et correspondent à la fin de la chaîne. Les caractères `^` et `$` garantissent que l’expression régulière établit une correspondance avec la totalité de la valeur du paramètre de route. Sans les caractères `^` et `$`, l’expression régulière peut correspondre à n’importe quelle sous-chaîne dans la chaîne, ce qui est souvent indésirable. Le tableau suivant contient des exemples et explique pourquoi ils établissent ou non une correspondance.

| Expression   | Chaîne    | Faire correspondre à | Commentaire               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | hello     | Oui   | Correspondances de sous-chaînes     |
| `[a-z]{2}`   | 123abc456 | Oui   | Correspondances de sous-chaînes     |
| `[a-z]{2}`   | mz        | Oui   | Correspondance avec l’expression    |
| `[a-z]{2}`   | MZ        | Oui   | Non-respect de la casse    |
| `^[a-z]{2}$` | hello     | Non    | Voir `^` et `$` ci-dessus |
| `^[a-z]{2}$` | 123abc456 | Non    | Voir `^` et `$` ci-dessus |

Pour plus d’informations sur la syntaxe des expressions régulières, consultez [Expressions régulières du .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Pour contraindre un paramètre à un ensemble connu de valeurs possibles, utilisez une expression régulière. Par exemple, `{action:regex(^(list|get|create)$)}` établit une correspondance avec la valeur de route `action` uniquement pour `list`, `get` ou `create`. Si elle est passée dans le dictionnaire de contraintes, la chaîne `^(list|get|create)$` est équivalente. Les contraintes passées dans le dictionnaire de contraintes (c’est-à-dire qui ne sont pas inline dans un modèle) qui ne correspondent pas à l’une des contraintes connues sont également traitées comme des expressions régulières.

## <a name="custom-route-constraints"></a>Contraintes d’itinéraire personnalisé

Outre les contraintes d’itinéraire intégré, les contraintes d’itinéraire personnalisé peuvent être créées en implémentant l’interface <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>. L’interface `IRouteConstraint` contient une méthode unique, `Match`, qui retourne `true` si la contrainte est satisfaite et `false` dans le cas contraire.

Pour utiliser un `IRouteConstraint` personnalisé, le type de contrainte d’itinéraire doit être inscrit avec le `RouteOptions.ConstraintMap` de l’application dans le conteneur de service de l’application. Un <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> est un dictionnaire qui mappe les clés de contrainte d’itinéraire aux implémentations `IRouteConstraint` qui valident ces contraintes. Le `RouteOptions.ConstraintMap` d’une application peut être mis à jour dans `Startup.ConfigureServices` dans le cadre d’un appel `services.AddRouting` ou en configurant `RouteOptions` directement avec `services.Configure<RouteOptions>`. Par exemple :

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

La contrainte peut ensuite être appliquée aux itinéraires de la manière habituelle, en utilisant le nom spécifié lors de l’inscription du type de contrainte. Par exemple :

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Informations de référence sur le transformateur de paramètre

Transformateurs de paramètre :

* Sont exécutés lors de la génération d’un lien pour un `Route`.
* Implémentez `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Sont configurés à l’aide de <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Prennent la valeur de routage du paramètre et la convertissent en une nouvelle valeur de chaîne.
* Aboutissent à l’utilisation de la valeur transformée dans le lien généré.

Par exemple, un transformateur de paramètre `slugify` personnalisé dans le modèle d’itinéraire `blog\{article:slugify}` avec `Url.Action(new { article = "MyTestArticle" })` génère `blog\my-test-article`.

Pour utiliser un transformateur de paramètre dans un modèle d’itinéraire, configurez-le d’abord en utilisant <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> dans `Startup.ConfigureServices` :

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

Les transformateurs de paramètre sont utilisés par le framework pour transformer l’URI où un point de terminaison est résolu. Par exemple, ASP.NET Core MVC utilise des transformateurs de paramètre pour convertir la valeur de routage utilisée et la faire correspondre à un `area`, `controller`, `action` et `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

Avec la route précédente, l’action `SubscriptionManagementController.GetAll()` est mise en correspondance avec l’URI `/subscription-management/get-all`. Un transformateur de paramètre ne modifie pas les valeurs de routage utilisées pour générer un lien. Par exemple, `Url.Action("GetAll", "SubscriptionManagement")` produit `/subscription-management/get-all`.

ASP.NET Core fournit des conventions d’API pour l’utilisation des transformateurs de paramètre avec des routages générés :

* La convention de l’API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` est fournie avec ASP.NET Core MVC. Cette convention applique un transformateur de paramètre spécifié à tous les routages d’attributs dans l’application. Le transformateur de paramètre transforme les jetons de routage d’attribut quand ils sont remplacés. Pour plus d’informations, consultez [Utiliser un transformateur de paramètre pour personnaliser le remplacement des jetons](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* Razor Pages a la convention d’API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention`. Cette convention applique un transformateur de paramètre spécifié à toutes les pages Razor découvertes automatiquement. Le transformateur de paramètre transforme les segments du nom de dossier et du nom de fichier des routes Razor Pages. Pour plus d’informations, consultez [Utiliser un transformateur de paramètre pour personnaliser les routages de pages](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Informations de référence sur la génération d’URL

L’exemple suivant montre comment générer un lien vers une route selon un dictionnaire de valeurs de route et un <xref:Microsoft.AspNetCore.Routing.RouteCollection> données.

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

Le `VirtualPath` généré à la fin de l’exemple précédent est `/package/create/123`. Le dictionnaire fournit les valeurs de route `operation` et `id` du modèle « Suivi de package de route », `package/{operation}/{id}`. Pour plus d’informations, consultez l’exemple de code dans la section [Utilisation du middleware de routage](#use-routing-middleware) ou l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Le deuxième paramètre pour le constructeur `VirtualPathContext` est une collection de *valeurs ambiantes*. Les valeurs ambiantes sont pratiques à utiliser, car elles limitent le nombre de valeurs qu’un développeur doit spécifier dans un contexte de requête. Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens. Dans l’action `About` de `HomeController` d’une application ASP.NET Core MVC, vous n’avez pas besoin de spécifier la valeur de route du contrôleur pour créer un lien vers l’action `Index` : la valeur ambiante de `Home` est utilisée.

Les valeurs ambiantes qui ne correspondent pas à un paramètre sont ignorées. Les valeurs ambiantes sont également ignorées quand une valeur explicitement fournie remplace la valeur ambiante. La mise en correspondance se produit de gauche à droite dans l’URL.

Les valeurs fournies explicitement mais qui n’ont pas de correspondance avec un segment de la route sont ajoutées à la chaîne de requête. Le tableau suivant présente le résultat en cas d’utilisation du modèle de routage `{controller}/{action}/{id?}`.

| Valeurs ambiantes                     | Valeurs explicites                        | Résultat                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| controller = "Home"                | action = "About"                       | `/Home/About`           |
| controller = "Home"                | controller = "Order", action = "About" | `/Order/About`          |
| controller = "Home", color = "Red" | action = "About"                       | `/Home/About`           |
| controller = "Home"                | action = "About", color = "Red"        | `/Home/About?color=Red` |

Si une route a une valeur par défaut qui ne correspond pas à un paramètre et que cette valeur est explicitement fournie, elle doit correspondre à la valeur par défaut :

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La génération de liens génère un lien pour cette route seulement quand les valeurs correspondantes pour `controller` et pour `action` sont fournies.

## <a name="complex-segments"></a>Segments complexes

Les segments complexes (par exemple, `[Route("/x{token}y")]`) sont traités par la mise en correspondance des littéraux de droite à gauche de manière non gourmande. Consultez [ce code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) pour obtenir une explication détaillée de la façon dont les segments complexes sont mis en correspondance. [L’exemple de code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) n’est pas utilisé par ASP.NET Core, mais il fournit une bonne explication des segments complexes.
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->
