---
title: Routage dans ASP.NET Core
author: ardalis
description: Découvrez comment la fonctionnalité de routage ASP.NET Core est chargée du mappage d’une requête entrante à un gestionnaire de routage.
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2018
uid: fundamentals/routing
ms.openlocfilehash: 500cefbc7caee2054b4afda7c1277685862f5ad4
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348557"
---
# <a name="routing-in-aspnet-core"></a>Routage dans ASP.NET Core

Par [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) et [Rick Anderson](https://twitter.com/RickAndMSFT)

La fonctionnalité de routage est chargée du mappage d’une requête entrante à un gestionnaire de routage. Les routes sont définies dans l’application et configurées au démarrage de l’application. Une route peut éventuellement extraire des valeurs de l’URL contenue dans la requête, et ces valeurs peuvent ensuite être utilisées pour le traitement de la requête. À l’aide des informations de route fournies par l’application, la fonctionnalité de routage peut également générer des URL qui mappent à des gestionnaires de routage. Par conséquent, le routage peut trouver un gestionnaire de routage en fonction d’une URL ou l’URL correspondant à un gestionnaire de routage donné en fonction des informations du gestionnaire de routage.

> [!IMPORTANT]
> Ce document traite du routage ASP.NET Core de bas niveau. Pour plus d’informations sur le routage ASP.NET Core MVC, consultez <xref:mvc/controllers/routing>.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Concepts de base du routage

Le routage utilise des *routes* (implémentations de <xref:Microsoft.AspNetCore.Routing.IRouter>) pour :

* Mapper les requêtes entrantes à des *gestionnaires de routage*.
* Générer des URL utilisées dans les réponses.

En général, une application possède une collection de routes unique. Quand une requête arrive, la collection de routes est traitée dans l’ordre. La requête entrante recherche une route qui correspond à son URL en appelant la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> sur chaque route disponible dans la collection de routes. En revanche, une réponse peut utiliser le routage pour générer des URL (par exemple, pour la redirection ou les liens) en fonction des informations de route et éviter ainsi d’avoir à coder en dur des URL, ce qui facilite la maintenance.

Le routage est connecté au pipeline de [l’intergiciel (middleware)](xref:fundamentals/middleware/index) par la classe <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>. [ASP.NET Core MVC](xref:mvc/overview) ajoute le routage au pipeline de l’intergiciel dans le cadre de sa configuration. Pour en savoir plus sur l’utilisation du routage comme composant autonome, consultez la section [Utilisation du middleware de routage](#use-routing-middleware).

### <a name="url-matching"></a>Correspondance d’URL

La correspondance d’URL est le processus par lequel le routage distribue une requête entrante à un *gestionnaire*. Ce processus est basé sur des données présentes dans le chemin de l’URL, mais il peut être étendu pour prendre en compte toutes les données de la requête. La possibilité de distribuer des requêtes à des gestionnaires distincts est essentielle pour adapter la taille et la complexité d’une application.

Les requêtes entrantes entrent dans `RouterMiddleware`, qui appelle la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> sur chaque route dans l’ordre. L’instance <xref:Microsoft.AspNetCore.Routing.IRouter> détermine s’il faut *gérer* la requête en affectant à [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) un <xref:Microsoft.AspNetCore.Http.RequestDelegate> non null. Si une route définit un gestionnaire pour la requête, le traitement de la route s’arrête et le gestionnaire est appelé pour traiter la requête. Si toutes les routes sont tentées et qu’aucun gestionnaire n’est trouvé pour la requête, le middleware appelle *next* : le middleware suivant dans le pipeline de requête est alors appelé.

L’entrée principale de `RouteAsync` est le [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associé à la requête actuelle. Le `RouteContext.Handler` et [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) sont des sorties définies après la correspondance d’une route.

Une correspondance pendant `RouteAsync` définit également les propriétés de `RouteContext.RouteData` avec les valeurs appropriées en fonction du traitement de requête effectué jusqu’à présent. Si une route correspond à une requête, `RouteContext.RouteData` contient des informations d’état importantes concernant le *résultat*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) est un dictionnaire de *valeurs de route* produites à partir de la route. Ces valeurs sont généralement déterminées en décomposant l’URL en jetons. Elles peuvent être utilisées pour accepter l’entrée d’utilisateur ou pour prendre d’autres décisions relatives à la distribution à l’intérieur de l’application.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) est un conteneur de propriétés des données supplémentaires associées à la route mise en correspondance. Les `DataTokens` sont fournis pour prendre en charge l’association de données d’état à chaque route afin que l’application puisse prendre des décisions ultérieurement en fonction de la route avec laquelle la correspondance a été établie. Ces valeurs sont définies par le développeur et n’affectent **pas** le comportement du routage de quelque manière que ce soit. De plus, les valeurs dissimulées dans des jetons de données peuvent être de n’importe quel type, contrairement aux valeurs de route, qui doivent être facilement convertibles en chaînes et à partir de chaînes.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) est une liste des routes qui ont participé à la mise en correspondance correcte de la requête. Les routes peuvent être imbriquées les unes dans les autres. La propriété `Routers` reflète le chemin à travers l’arborescence logique des routes qui ont généré une correspondance. En général le premier élément de `Routers` est la collection de routes et il doit être utilisé pour la génération d’URL. Le dernier élément de `Routers` est le gestionnaire de routage avec lequel la correspondance a été établie.

### <a name="url-generation"></a>Génération d’URL

La génération d’URL est le processus par lequel le routage peut créer un chemin d’URL basé sur un ensemble de valeurs de route. Cela permet une séparation logique entre vos gestionnaires et les URL qui y accèdent.

La génération d’URL suit un processus itératif similaire, mais elle commence par un appel du code utilisateur ou de framework à la méthode <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> de la collection de routes. La méthode `GetVirtualPath` de chaque *route* est alors appelée en séquence jusqu’à ce qu’un <xref:Microsoft.AspNetCore.Routing.VirtualPathData> non null soit retourné.

Les entrées principales dans `GetVirtualPath` sont les suivantes :

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Les routes utilisent principalement les valeurs de route fournies par `Values` et `AmbientValues` pour décider où il est possible de générer une URL et les valeurs à inclure. Les `AmbientValues` sont l’ensemble des valeurs de route produites à partir de la mise en correspondance de la requête actuelle avec le système de routage. En revanche, les `Values` sont les valeurs de route qui spécifient la façon de générer l’URL souhaitée pour l’opération actuelle. `HttpContext` est fourni au cas où une route aurait besoin d’obtenir des services ou des données supplémentaires associés au contexte actuel.

> [!TIP]
> Considérez [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) comme un ensemble de substitutions pour [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*). La génération d’URL tente de réutiliser des valeurs de route de la requête actuelle afin de faciliter la génération d’URL pour des liens utilisant la même route ou les mêmes valeurs de route.

La sortie de `GetVirtualPath` est un `VirtualPathData`. `VirtualPathData` est l’équivalent de `RouteData`. `VirtualPathData` contient le `VirtualPath` de l’URL de sortie et des propriétés supplémentaires qui doivent être définies par la route.

La propriété [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) contient le *chemin virtuel* produit par la route. Selon vos besoins, vous devrez peut-être traiter plus avant le chemin. Si vous souhaitez afficher l’URL générée au format HTML, ajoutez un préfixe au chemin de base de l’application.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) est une référence à la route qui a généré avec succès l’URL.

La propriété [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) est un dictionnaire de données supplémentaires relatives à la route qui a généré l’URL. Il s’agit de l’équivalent de [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Création de routes

Le routage fournit la classe <xref:Microsoft.AspNetCore.Routing.Route> comme implémentation standard d’<xref:Microsoft.AspNetCore.Routing.IRouter>. `Route` utilise la syntaxe de *modèle de routage* pour définir les modèles qui correspondent au chemin d’URL quand <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> est appelé. `Route` utilise le même modèle de routage pour générer une URL quand `GetVirtualPath` est appelé.

La plupart des applications créent des routes en appelant <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> ou l’une des méthodes d’extension similaires définies sur <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>. Toutes ces méthodes créent une instance de <xref:Microsoft.AspNetCore.Routing.Route> et l’ajoutent à la collection de routes.

`MapRoute` ne prend pas de paramètre de gestionnaire de routage. `MapRoute` ajoute uniquement des routes gérées par <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>. Étant donné que le gestionnaire par défaut est un `IRouter`, il peut décider de ne pas traiter la requête. Par exemple, ASP.NET Core MVC est généralement configuré comme gestionnaire par défaut qui gère uniquement les requêtes correspondant à un contrôleur et une action disponibles. Pour en savoir plus sur le routage vers MVC, consultez <xref:mvc/controllers/routing>.

L’exemple de code suivant est un exemple d’appel à `MapRoute` utilisé par une définition de route ASP.NET Core MVC classique :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Ce modèle établit une correspondance avec un chemin d’URL comme `/Products/Details/17`, et extrait les valeurs de route `{ controller = Products, action = Details, id = 17 }`. Les valeurs de route sont déterminées par le fractionnement du chemin d’URL en segments et la mise en correspondance de chaque segment avec le nom des *paramètres de routage* dans le modèle de routage. Les paramètres de routage sont nommés. Ils sont définis en plaçant le nom du paramètre entre accolades `{ ... }`.

Le modèle précédent peut également mettre en correspondance le chemin d’URL `/` et produirait les valeurs `{ controller = Home, action = Index }`. Cela s’explique par le fait que les paramètres de routage `{controller}` et `{action}` ont des valeurs par défaut et que le paramètre de routage `id` est facultatif. Un signe égal `=` suivi d’une valeur après le nom du paramètre de routage définit une valeur par défaut pour le paramètre. Un point d’interrogation `?` après le nom du paramètre de routage définit le paramètre comme facultatif. Les paramètres de routage ayant une valeur par défaut produisent *toujours* une valeur de routage quand la route correspond. Les paramètres facultatifs ne produisent pas de valeur de routage si aucun segment de chemin d’URL ne correspondait.

Pour obtenir une description complète des fonctionnalités et de la syntaxe des modèles de routage, consultez [Informations de référence sur les modèles de routage](#route-template-reference).

Cet exemple inclut une *contrainte de routage* :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Ce modèle établit une correspondance avec un chemin d’URL comme `/Products/Details/17`, mais pas `/Products/Details/Apples`. La définition de paramètre de routage `{id:int}` définit une [contrainte de routage](#route-constraint-reference) pour le paramètre de routage `id`. Les contraintes de routage implémentent <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> et inspectent les valeurs de route pour les vérifier. Dans cet exemple, la valeur de route `id` doit être convertible en entier. Pour obtenir une explication plus détaillée des contraintes de routage fournies par le framework, consultez [Informations de référence sur les contraintes de routage](#route-constraint-reference).

Des surcharges supplémentaires de `MapRoute` acceptent des values pour `constraints`, `dataTokens` et `defaults`. Ces paramètres supplémentaires de `MapRoute` sont définis comme type `object`. L’utilisation classique de ces paramètres consiste à passer un objet typé anonymement, où les noms des propriétés du type anonyme correspondent aux noms de paramètre de routage.

Les deux exemples suivants créent des routes équivalentes :

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
> La syntaxe inline pour la définition des contraintes et des valeurs par défaut peut être pratique pour les routes simples. Toutefois, certaines fonctionnalités comme les jetons de données ne sont pas prises en charge par la syntaxe inline.

L’exemple suivant illustre quelques autres scénarios :

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Ce modèle établit une correspondance avec un chemin d’URL comme `/Blog/All-About-Routing/Introduction`, et extrait les valeurs `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`. Les valeurs de route par défaut pour `controller` et `action` sont produites par la route, même s’il n’existe aucun paramètre de routage correspondant dans le modèle. Il est possible de spécifier des valeurs par défaut dans le modèle de route. Le paramètre de routage `article` est défini comme un *fourre-tout*  par la présence d’un astérisque `*` avant le nom de paramètre de routage. Les paramètres de routage fourre-tout capturent le reste du chemin d’URL et peuvent également établir une correspondance avec la chaîne vide.

Cet exemple ajoute des contraintes de routage et des jetons de données :

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Ce modèle établit une correspondance avec un chemin d’URL comme `/en-US/Products/5`, et extrait les valeurs `{ controller = Products, action = Details, id = 5 }` et les jetons de données `{ locale = en-US }`.

![Jetons Windows de variables locales](routing/_static/tokens.png)

### <a name="url-generation"></a>Génération d’URL

La classe `Route` peut également effectuer une génération d’URL en combinant un ensemble de valeurs de route et son modèle de routage. Il s’agit logiquement du processus inverse de la mise en correspondance du chemin d’URL.

> [!TIP]
> Pour mieux comprendre la génération d’URL, imaginez l’URL que vous voulez générer, puis pensez à la façon dont un modèle de routage établirait une correspondance avec cette URL. Quelles valeurs seraient produites ? Cela équivaut approximativement à la façon dont la génération d’URL fonctionne dans la classe `Route`.

Cet exemple utilise une route de style ASP.NET Core MVC de base :

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Avec les valeurs de route `{ controller = Products, action = List }`, cette route génère l’URL `/Products/List`. Les valeurs de route remplacent les paramètres de routage correspondant pour former le chemin d’URL. Étant donné qu’`id` est un paramètre de routage facultatif, ce n’est pas un problème qu’il n’ait pas de valeur.

Avec les valeurs de route `{ controller = Home, action = Index }`, cette route génère l’URL `/`. Les valeurs de route fournies sont mises en correspondance avec les valeurs par défaut afin que les segments correspondant à ces valeurs peuissent être omis sans risque. Les deux URL générées effectuent un aller-retour avec cette définition de route, et produisent les mêmes valeurs de route que celles utilisées pour générer l’URL.

> [!TIP]
> Pour générer des URL, une application utilisant ASP.NET Core MVC doit utiliser <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> au lieu d’effectuer un appel directement dans le routage.

Pour plus d’informations sur la génération d’URL, consultez [Informations de référence sur la génération d’URL](#url-generation-reference).

## <a name="use-routing-middleware"></a>Utilisation du middleware de routage

::: moniker range=">= aspnetcore-2.1"

Référencez le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) dans le fichier projet de l’application.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Référencez le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage) dans le fichier projet de l’application.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Référencez [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) dans le fichier projet de l’application.

::: moniker-end

Ajoutez le routage au conteneur de service dans `Startup.ConfigureServices` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Les routes doivent être configurées dans la méthode `Startup.Configure`. L’exemple d’application utilise ces API :

* `RouteBuilder`
* `Build`
* `MapGet` &ndash; Établit une correspondance uniquement avec les requêtes HTTP GET.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

Le tableau suivant montre les réponses avec les URI donnés.

| URI                  | Réponse                                          |
| -------------------- | ------------------------------------------------- |
| /package/create/3    | Hello! Valeurs de route : [operation, create], [id, 3] |
| /package/track/-3    | Hello! Valeurs de route : [operation, track], [id, -3] |
| /package/track/-3/   | Hello! Valeurs de route : [operation, track], [id, -3] |
| /package/track/      | &lt;Fourre-tout, aucune correspondance&gt;                    |
| GET /hello/Joe       | Hi, Joe!                                          |
| POST /hello/Joe      | &lt;Fourre-tout, établir une correspondance avec HTTP GET uniquement&gt;       |
| GET /hello/Joe/Smith | &lt;Fourre-tout, aucune correspondance&gt;                    |

Si vous configurez une seule route, appelez <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> en passant une instance `IRouter`. Vous n’avez pas besoin d’utiliser <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.

Le framework fournit un ensemble de méthodes d’extension pour la création de routes, notamment :

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Certaines de ces méthodes, comme `MapGet`, nécessitent qu’un `RequestDelegate` soit fourni. `RequestDelegate` est utilisé comme *gestionnaire de routage* quand une correspondance est trouvée pour la route. D’autres méthodes de cette famille permettent de configurer un pipeline de middleware utilisé comme gestionnaire de routage. Si la méthode *Map* n’accepte pas de gestionnaire, par exemple `MapRoute`, elle utilise <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

Les méthodes `Map[Verb]` utilisent des contraintes pour limiter la route au verbe HTTP dans le nom de la méthode. Par exemple, consultez <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> et <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Informations de référence sur les modèles de routage

Les jetons placés entre accolades (`{ ... }`) définissent des *paramètres de routage* qui sont liés si une correspondance est trouvée pour la route. Vous pouvez définir plusieurs paramètres de routage dans un segment de route, mais ils doivent être séparés par une valeur littérale. Par exemple `{controller=Home}{action=Index}` n’est pas une route valide, car il n’y a aucune valeur littérale entre `{controller}` et `{action}`. Ces paramètres de routage doivent avoir un nom, et ils autorisent la spécification d’attributs supplémentaires.

Un texte littéral autre que les paramètres de routage (par exemple, `{id}`) et le séparateur de chemin `/` doit correspondre au texte présent dans l’URL. La correspondance de texte ne respecte pas la casse et est basée sur la représentation décodée du chemin des URL. Pour mettre en correspondance le délimiteur de paramètre de routage littéral `{` ou `}`, placez-le dans une séquence d’échappement en répétant le caractère (`{{` ou `}}`).

Les modèles d’URL qui tentent de capturer un nom de fichier avec une extension de fichier facultative font l’objet de considérations supplémentaires. Prenez par exemple le modèle `files/{filename}.{ext?}`. Quand `filename` et `ext` existe tous deux, les deux valeurs sont remplies. Si seul `filename` existe dans l’URL, une correspondance est trouvée pour la route, car le point final `.` est facultatif. Les URL suivantes correspondent à cette route :

* `/files/myFile.txt`
* `/files/myFile`

Vous pouvez utiliser le caractère `*` comme préfixe d’un paramètre de routage à lier au reste de l’URI. Cela s’appelle un paramètre *fourre-tout*. Par exemple, `blog/{*slug}` établit une correspondance avec n’importe quel URI commençant par `/blog` et suivi de n’importe quelle valeur (affectée à la valeur de route `slug`). Les paramètres fourre-tout peuvent également établir une correspondance avec la chaîne vide.

::: moniker range=">= aspnetcore-2.2"

Le paramètre fourre-tout place les caractères appropriés dans une séquence d’échappement lorsque la route est utilisée pour générer une URL, y compris les caractères de séparation de chemin (`/`). Par exemple, la route `foo/{*path}` avec les valeurs de route `{ path = "my/path" }` génère `foo/my%2Fpath`. Notez la barre oblique d’échappement. Pour les séparateurs de chemin aller-retour, utilisez le préfixe de paramètre de routage `**`. La route `foo/{**path}` avec `{ path = "my/path" }` génère `foo/my/path`.

::: moniker-end

Les paramètres de routage peuvent avoir des *valeurs par défaut*, désignées en spécifiant la valeur par défaut après le nom du paramètre, séparés par un signe égal (`=`). Par exemple, `{controller=Home}` définit `Home` comme valeur par défaut de `controller`. La valeur par défaut est utilisée si aucune valeur n’est présente dans l’URL pour le paramètre. En plus des valeurs par défaut, les paramètres de routage peuvent être facultatifs, spécifiés en ajoutant un point d’interrogation (`?`) à la fin du nom du paramètre, comme dans `id?`. La différence entre les valeurs facultatives et les paramètres de routage par défaut est qu’un paramètre de routage ayant une valeur par défaut produit toujours une valeur, tandis qu’un paramètre facultatif a une valeur uniquement quand une valeur est fournie par l’URL de requête.

::: moniker range=">= aspnetcore-2.2"

Les paramètres de routage peuvent avoir des contraintes, qui doivent établir une correspondance avec la valeur de route liée à partir de l’URL. L’ajout d’un signe deux-points (`:`) et d’un nom de contrainte après le nom du paramètre de routage spécifie une *contrainte inline* sur un paramètre de routage. Si la contrainte exige des arguments, ils sont fournis entre parenthèses `( )` après le nom de la contrainte. Il est possible de spécifier plusieurs contraintes inline en ajoutant un autre signe deux-points (`:`) et le nom d’une autre contrainte. Le nom de la contrainte et les arguments sont passés au service <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> pour créer une instance de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> à utiliser dans le traitement des URL. Si le constructeur de contrainte nécessite des services, ils sont résolus à partir des services d’application de l’injection de dépendance. Par exemple, le modèle de routage `blog/{article:minlength(10)}` spécifie une contrainte `minlength` avec l’argument `10`. Pour plus d’informations sur les contraintes de routage et pour obtenir la liste des contraintes fournies par le framework, consultez la section [Informations de référence sur les contraintes de routage](#route-constraint-reference).

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Les paramètres de routage peuvent avoir des contraintes, qui doivent établir une correspondance avec la valeur de route liée à partir de l’URL. L’ajout d’un signe deux-points (`:`) et d’un nom de contrainte après le nom du paramètre de routage spécifie une *contrainte inline* sur un paramètre de routage. Si la contrainte exige des arguments, ils sont fournis entre parenthèses `( )` après le nom de la contrainte. Il est possible de spécifier plusieurs contraintes inline en ajoutant un autre signe deux-points (`:`) et le nom d’une autre contrainte. Le nom de la contrainte et les arguments sont passés au service <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> pour créer une instance de <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> à utiliser dans le traitement des URL. Par exemple, le modèle de routage `blog/{article:minlength(10)}` spécifie une contrainte `minlength` avec l’argument `10`. Pour plus d’informations sur les contraintes de routage et pour obtenir la liste des contraintes fournies par le framework, consultez la section [Informations de référence sur les contraintes de routage](#route-constraint-reference).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Les paramètres de routage peuvent également contenir des transformateurs de paramètre, lesquels transforment une valeur de paramètre lors de la génération des liens et de la mise en correspondance des actions et des pages avec les URI. À l’instar des contraintes, les transformateurs de paramètre peuvent être ajoutés inline à un paramètre de routage en ajoutant un signe deux-points (`:`) et le nom du transformateur après le nom du paramètre de routage. Par exemple, le modèle de routage `blog/{article:slugify}` spécifie un transformateur `slugify`.

::: moniker-end

Le tableau suivant illustre certains modèles de routage et leur comportement.

| Modèle de routage                         | Exemple d’URL correspondante  | Notes                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| hello                                  | /hello                | Correspond uniquement au chemin unique `/hello`                                  |
| {Page=Home}                            | /                     | Correspond à `Page` et le définit sur `Home`                                      |
| {Page=Home}                            | /Contact              | Correspond à `Page` et le définit sur `Contact`                                   |
| {controller}/{action}/{id?}            | /Products/List        | Correspond au contrôleur `Products` et à l’action `List`                       |
| {controller}/{action}/{id?}            | /Products/Details/123 |  Correspond au contrôleur `Products` et à l’action `Details`.  `id` a la valeur 123 |
| {controller=Home}/{action=Index}/{id?} | /                     |  Correspond au contrôleur `Home` et à la méthode `Index`. `id` est ignoré.       |

L’utilisation d’un modèle est généralement l’approche la plus simple pour le routage. Il est également possible de spécifier des contraintes et des valeurs par défaut hors du modèle de routage.

> [!TIP]
> Activez la [journalisation](xref:fundamentals/logging/index) pour voir comment les implémentations de routage intégrées, comme `Route`, établissent des correspondances avec des requêtes.

## <a name="reserved-routing-names"></a>Noms de routage réservés

Les mots clés suivants sont des noms réservés qui ne peuvent pas être utilisés comme paramètres ou noms de routage :

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Informations de référence sur les contraintes de routage

Les contraintes de routage s’exécutent quand un `Route` correspond à la syntaxe de l’URL entrante et a décomposé le chemin de l’URL en valeurs de route. En général, les contraintes de routage inspectent la valeur de route associée par le biais du modèle de routage, et créent une décision oui/non indiquant si la valeur est, ou non, acceptable. Certaines contraintes de routage utilisent des données hors de la valeur de route pour déterminer si la requête peut être routée. Par exemple, <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> peut accepter ou rejeter une requête en fonction de son verbe HTTP.

> [!WARNING]
> Évitez d’utiliser des contraintes pour la **validation d’entrée**, car les entrées non valides génèrent alors une réponse *404 - Introuvable* au lieu d’une réponse *400 - Requête incorrecte* avec un message d’erreur approprié. Les contraintes de routage servent à **lever l’ambiguïté** entre des routes similaires, et non à valider les entrées d’une route particulière.

Le tableau suivant illustre certaines contraintes de routage et leur comportement attendu.

| contrainte | Exemple | Exemples de correspondances | Notes |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Correspond à n’importe quel entier |
| `bool` | `{active:bool}` | `true`, `FALSE` | Correspond à `true` ou à `false` (non-respect de la casse) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Correspond à une valeur `DateTime` valide (dans la culture invariante ; voir l’avertissement) |
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
| `required` | `{name:required}` | `Rick` |  Utilisé pour garantir qu’une valeur autre qu’un paramètre est présente pendant la génération de l’URL |

Il est possible d’appliquer plusieurs contraintes séparées par un point-virgule à un même paramètre. Par exemple, la contrainte suivante limite un paramètre à une valeur entière supérieure ou égale à 1 :

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Les contraintes de routage qui vérifient que l’URL peut être convertie en type CLR (comme `int` ou `DateTime`) utilisent toujours la culture invariant. ces contraintes partent du principe que l’URL n’est pas localisable. Les contraintes de routage fournies par le framework ne modifient pas les valeurs stockées dans les valeurs de route. Toutes les valeurs de route analysées à partir de l’URL sont stockées sous forme de chaînes. Par exemple, la contrainte `float` tente de convertir la valeur de route en valeur float, mais la valeur convertie est utilisée uniquement pour vérifier qu’elle peut être convertie en valeur float.

## <a name="regular-expressions"></a>Expressions régulières

Le framework ASP.NET Core ajoute `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` au constructeur d’expression régulière. Pour obtenir une description de ces membres, consultez <xref:System.Text.RegularExpressions.RegexOptions>.

Les expressions régulières utilisent les délimiteurs et des jetons semblables à ceux utilisés par le service de routage et le langage C#. Les jetons d’expression régulière doivent être placés dans une séquence d’échappement. Pour que vous puissiez utiliser l’expression régulière `^\d{3}-\d{2}-\d{4}$` dans le service de routage, il faut qu’elle ait les caractères `\` tapés sous la forme `\\` dans le fichier source C# afin de placer dans une séquence d’échappement le caractère d’échappement de chaîne `\` (sauf en cas d’utilisation de [littéraux de chaîne textuelle](/dotnet/csharp/language-reference/keywords/string). Les caractères `{`, `}`, `[` et `]` doivent être placés en double dans une séquence d’échappement, afin de placer les délimiteurs de paramètre de routage dans une séquence d’échappement. Le tableau ci-dessous montre une expression régulière et la version placée dans une séquence d’échappement.

| Expression            | Dans une séquence d’échappement                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Souvent, les expressions régulières utilisées dans le routage commencent par le caractère `^` (position de départ de correspondance de la chaîne) et se terminent par le caractère `$` (position de fin de correspondance de la chaîne). Les caractères `^` et `$` garantissent que l’expression régulière établit une correspondance avec la totalité de la valeur du paramètre de route. Sans les caractères `^` et `$`, l’expression régulière peut correspondre à n’importe quelle sous-chaîne dans la chaîne, ce qui est souvent indésirable. Le tableau ci-dessous présente des exemples et explique pourquoi ils établissent, ou non, une correspondance.

| Expression   | Chaîne    | Faire correspondre à | Commentaire               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | hello     | Oui   | Correspondances de sous-chaînes     |
| `[a-z]{2}`   | 123abc456 | Oui   | Correspondances de sous-chaînes     |
| `[a-z]{2}`   | mz        | Oui   | Correspondance avec l’expression    |
| `[a-z]{2}`   | MZ        | Oui   | Non-respect de la casse    |
| `^[a-z]{2}$` |  hello    | Non    | Voir `^` et `$` ci-dessus |
| `^[a-z]{2}$` | 123abc456 | Non    | Voir `^` et `$` ci-dessus |

Pour plus d’informations sur la syntaxe des expressions régulières, consultez [Expressions régulières du .NET Framework](/dotnet/standard/base-types/regular-expression-language-quick-reference).

Pour contraindre un paramètre à un ensemble connu de valeurs possibles, utilisez une expression régulière. Par exemple, `{action:regex(^(list|get|create)$)}` établit une correspondance avec la valeur de route `action` uniquement pour `list`, `get` ou `create`. Si elle est passée dans le dictionnaire de contraintes, la chaîne `^(list|get|create)$` est équivalente. Les contraintes passées dans le dictionnaire de contraintes (c’est-à-dire qui ne sont pas inline dans un modèle) qui ne correspondent pas à l’une des contraintes connues sont également traitées comme des expressions régulières.

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a>Informations de référence sur le transformateur de paramètre

Transformateurs de paramètre :

* Sont exécutés lors de la génération d’un lien pour un `Route`.
* Implémentez `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.
* Sont configurés à l’aide de <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.
* Prennent la valeur de routage du paramètre et la convertissent en une nouvelle valeur de chaîne.
* La valeur transformée est utilisée dans le lien généré.

Par exemple, un transformateur de paramètre `slugify` personnalisé dans le modèle d’itinéraire `blog\{article:slugify}` avec `Url.Action(new { article = "MyTestArticle" })` génère `blog\my-test-article`.

Les transformateurs de paramètre sont également utilisés par les frameworks pour transformer l’URI vers lequel un point de terminaison est résolu. Par exemple, ASP.NET Core MVC utilise des transformateurs de paramètre pour convertir la valeur de routage utilisée et la faire correspondre à un `area`, `controller`, `action` et `page`.

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

Avec la route précédente, l’action `SubscriptionManagementController.GetAll()` est mise en correspondance avec l’URI `/subscription-management/get-all`. Un transformateur de paramètre ne modifie pas les valeurs de routage utilisées pour générer un lien. `Url.Action("GetAll", "SubscriptionManagement")` obtient en sortie `/subscription-management/get-all`.

ASP.NET Core fournit des conventions d’API pour l’utilisation des transformateurs de paramètre avec des routages générés :

* La convention de l’API `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` est fournie avec ASP.NET Core MVC. Cette convention applique un transformateur de paramètre spécifié à tous les routages d’attributs dans l’application. Le transformateur de paramètre transforme les jetons de routage d’attribut quand ils sont remplacés. Pour plus d’informations, consultez [Utiliser un transformateur de paramètre pour personnaliser le remplacement des jetons](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).
* La convention de l’API `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` est fournie avec les pages Razor. Cette convention applique un transformateur de paramètre spécifié à toutes les pages Razor découvertes automatiquement. Le transformateur de paramètre transforme les segments du nom de dossier et du nom de fichier des routages de pages Razor. Pour plus d’informations, consultez [Utiliser un transformateur de paramètre pour personnaliser les routages de pages](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).

::: moniker-end

## <a name="url-generation-reference"></a>Informations de référence sur la génération d’URL

L’exemple suivant montre comment générer un lien vers une route selon un dictionnaire de valeurs de route et un <xref:Microsoft.AspNetCore.Routing.RouteCollection> données.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

Le `VirtualPath` généré à la fin de l’exemple précédent est `/package/create/123`. Le dictionnaire fournit les valeurs de route `operation` et `id` du modèle « Suivi de package de route », `package/{operation}/{id}`. Pour plus d’informations, consultez l’exemple de code dans la section [Utilisation du middleware de routage](#use-routing-middleware) ou l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Le deuxième paramètre pour le constructeur `VirtualPathContext` est une collection de *valeurs ambiantes*. Les valeurs ambiantes offre beaucoup de souplesse en limitant le nombre de valeurs qu’un développeur doit spécifier dans un contexte de requête donné. Les valeurs de route actuelles de la requête actuelle sont considérées comme des valeurs ambiantes pour la génération de liens. Dans une application ASP.NET Core MVC, si vous êtes dans l’action `About` de `HomeController`, vous n’avez pas besoin de spécifier la valeur de route du contrôleur pour créer un lien vers l’action `Index` (la valeur ambiante de `Home` est utilisée).

Les valeurs ambiantes qui ne correspondent pas à un paramètre sont ignorées. De plus, les valeurs ambiantes sont également ignorées quand une valeur fournie explicitement le remplace, en procédant de gauche à droite dans l’URL.

Les valeurs qui sont fournis explicitement mais qui n’ont pas de correspondance sont ajoutées à la chaîne de requête. Le tableau suivant présente le résultat en cas d’utilisation du modèle de routage `{controller}/{action}/{id?}`.

| Valeurs ambiantes                | Valeurs explicites                   | Résultat                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| controller="Home"             | action="About"                    | `/Home/About`           |
| controller="Home"             | controller="Order",action="About" | `/Order/About`          |
| controller="Home",color="Red" | action="About"                    | `/Home/About`           |
| controller="Home"             | action="About",color="Red"        | `/Home/About?color=Red` |

Si une route a une valeur par défaut qui ne correspond pas à un paramètre et que cette valeur est explicitement fournie, elle doit correspondre à la valeur par défaut :

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

La génération de liens génère un lien pour cette route uniquement quand les valeurs correspondantes pour le contrôleur et l’action sont fournies.
