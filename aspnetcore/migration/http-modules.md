---
title: Migrer des modules et gestionnaires HTTP vers l’intergiciel (middleware) ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209844"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>Migrer des modules et gestionnaires HTTP vers l’intergiciel (middleware) ASP.NET Core

Par [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

Cet article explique comment migrer d’ASP.NET existants [modules HTTP et des gestionnaires dans system.webserver](/iis/configuration/system.webserver/) vers ASP.NET Core [intergiciel (middleware)](xref:fundamentals/middleware/index).

## <a name="modules-and-handlers-revisited"></a>Modules et gestionnaires revisitées

Avant de passer à l’intergiciel (middleware) ASP.NET Core, nous allons tout d’abord revoir le fonctionnement des gestionnaires et modules HTTP :

![Gestionnaire de modules](http-modules/_static/moduleshandlers.png)

**Les gestionnaires sont :**

* Classes qui implémentent [IHttpHandler](/dotnet/api/system.web.ihttphandler)

* Utilisé pour gérer les demandes avec un nom de fichier donné ou une extension, tel que *report*

* [Configuré](/iis/configuration/system.webserver/handlers/) dans *Web.config*

**Les modules sont :**

* Classes qui implémentent [IHttpModule](/dotnet/api/system.web.ihttpmodule)

* Appelé pour chaque demande

* En mesure de court-circuit (interrompre le traitement d’une demande)

* Ajouter à la réponse HTTP, ou créer leurs propres

* [Configuré](/iis/configuration/system.webserver/modules/) dans *Web.config*

**L’ordre dans lequel les modules traitent les requêtes entrantes est déterminé par :**

1. Le [cycle de vie d’application](https://msdn.microsoft.com/library/ms227673.aspx), c'est-à-dire les événements déclenchés par ASP.NET en une série : [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Chaque module peut créer un gestionnaire pour un ou plusieurs événements.

2. Pour le même événement, l’ordre dans lequel ils sont configurés dans *Web.config*.

En plus des modules, vous pouvez ajouter des gestionnaires pour les événements de cycle de vie à votre *Global.asax.cs* fichier. Ces gestionnaires s’exécutent après les gestionnaires dans les modules configurés.

## <a name="from-handlers-and-modules-to-middleware"></a>À partir des gestionnaires et des modules à l’intergiciel (middleware)

**Intergiciel (middleware) sont plus simples que les gestionnaires et modules HTTP :**

* Modules, gestionnaires, *Global.asax.cs*, *Web.config* (à l’exception de la configuration d’IIS) et le cycle de vie d’application ont disparu

* Les rôles des modules et gestionnaires ont été prises en charge par l’intergiciel (middleware)

* Intergiciel (middleware) sont configurés à l’aide de code plutôt que dans *Web.config*

* [Création de branches de pipeline](xref:fundamentals/middleware/index#use-run-and-map) vous permet d’envoyer les demandes au middleware spécifique, basé sur non seulement l’URL, mais également sur les en-têtes de demande, les chaînes de requête, etc.

**Intergiciel (middleware) sont très similaires aux modules :**

* Appelé en principe pour chaque demande

* En mesure de court-circuiter une demande, par [ne pas passer la demande à l’intergiciel suivant](#http-modules-shortcircuiting-middleware)

* En mesure de créer leur propre réponse HTTP

**Intergiciel (middleware) et les modules sont traités dans un ordre différent :**

* Ordre des intergiciels (middleware) est basé sur l’ordre dans lequel ils sont insérés dans le pipeline de demande, tandis que l’ordre des modules est principalement basé sur [cycle de vie d’application](https://msdn.microsoft.com/library/ms227673.aspx) événements

* Ordre des intergiciels (middleware) pour les réponses est l’inverse de celui pour les demandes, tandis que l’ordre des modules est le même pour les demandes et réponses

* Consultez [créer un pipeline d’intergiciel (middleware) avec IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![Intergiciel (middleware)](http-modules/_static/middleware.png)

Notez comment, dans l’image ci-dessus, le middleware d’authentification court-circuitée la demande.

## <a name="migrating-module-code-to-middleware"></a>Migration de code de module à l’intergiciel (middleware)

Un module HTTP existant doit ressembler à ceci :

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

Comme indiqué dans le [intergiciel (middleware)](xref:fundamentals/middleware/index) page, un intergiciel (middleware) ASP.NET Core est une classe qui expose un `Invoke` prise de méthode un `HttpContext` et en retournant un `Task`. Votre intergiciel (middleware) nouvelle ressemblera à ceci :

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

Le modèle d’intergiciel (middleware) précédent a été effectuée à partir de la section sur [écriture intergiciel (middleware)](xref:fundamentals/middleware/write).

Le *MyMiddlewareExtensions* classe d’assistance rend plus facile à configurer votre intergiciel (middleware) dans votre `Startup` classe. Le `UseMyMiddleware` méthode ajoute la classe du middleware au pipeline de requête. Services requis par l’intergiciel (middleware) injectés dans le constructeur du middleware.

<a name="http-modules-shortcircuiting-middleware"></a>

Votre module peut mettre fin à une demande, par exemple, si l’utilisateur n’est pas autorisé :

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

Un intergiciel (middleware) gère cela en appelant ne pas `Invoke` sur l’intergiciel suivant dans le pipeline. N’oubliez pas que cela n’interrompt pas entièrement la demande, car il se peut qu’intergiciels précédente sera toujours appelé lorsque la réponse fait via le pipeline.

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

Lorsque vous migrez des fonctionnalités de votre module vers votre nouveau intergiciel, peut-être estimez-vous que votre code ne se compile pas, car le `HttpContext` classe a considérablement changé dans ASP.NET Core. [Par la suite](#migrating-to-the-new-httpcontext), vous verrez comment migrer à nouveau ASP.NET Core HttpContext.

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>Migration insertion de module dans le pipeline de demande

Les modules HTTP sont généralement ajoutés au pipeline de demande à l’aide *Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

Convertir en [ajoutant votre intergiciel (middleware) nouvelle](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) au pipeline de requête dans votre `Startup` classe :

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

La position exacte dans le pipeline où vous insérez votre intergiciel (middleware) nouvelle dépend de l’événement qui elle est gérée en tant que module (`BeginRequest`, `EndRequest`, etc.) et son ordre dans la liste des modules dans *Web.config*.

Comme précédemment ne mentionné, il existe aucun cycle de vie d’application dans ASP.NET Core et l’ordre dans lequel les réponses sont traités par un intergiciel (middleware) diffère de l’ordre utilisé par les modules. Cela peut rendre votre décision de classement plus difficile.

Si le tri devient un problème, vous pouvez fractionner votre module en plusieurs composants d’intergiciel (middleware) qui peuvent être triés indépendamment.

## <a name="migrating-handler-code-to-middleware"></a>Migration du code gestionnaire vers l’intergiciel (middleware)

Un gestionnaire HTTP ressemble à ceci :

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

Dans votre projet ASP.NET Core, vous se traduirait cela un intergiciel (middleware) similaire à ceci :

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

Cet intergiciel est très similaire à l’intergiciel (middleware) correspondant aux modules. La seule différence est qu’ici aucun appel n’est à `_next.Invoke(context)`. C’est logique, car le gestionnaire n’est à la fin du pipeline de demande, il y aura aucun intergiciel (middleware) suivant pour appeler.

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>Migration insertion de gestionnaire dans le pipeline de demande

Configuration d’un gestionnaire HTTP est effectuée *Web.config* et ressemble à ceci :

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

Vous pouvez le convertir en ajoutant votre nouvel intergiciel (middleware) gestionnaire au pipeline de requête dans votre `Startup` (classe), similaire à l’intergiciel (middleware) converti à partir de modules. Le problème avec cette approche est qu’il enverrait ainsi toutes les demandes de votre nouvel intergiciel (middleware) gestionnaire. Toutefois, vous seulement souhaitez que les demandes avec une extension donnée pour atteindre votre intergiciel (middleware). Qui permet d’obtenir les mêmes fonctionnalités que vous aviez avec votre gestionnaire HTTP.

Une solution consiste à créer une branche le pipeline pour les requêtes avec une extension donnée, à l’aide de la `MapWhen` méthode d’extension. Cela dans le même `Configure` méthode où vous ajoutez d’autres intergiciels (middleware) :

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` utilise ces paramètres :

1. Une expression lambda qui prend le `HttpContext` et retourne `true` si la requête doit atteindre la branche. Cela signifie que vous pouvez créer des requêtes non seulement en fonction de leur extension, mais également sur les en-têtes de demande, les paramètres de chaîne de requête, etc. une branche.

2. Une expression lambda qui prend un `IApplicationBuilder` et ajoute tous les intergiciels (middleware) pour la branche. Cela signifie que vous pouvez ajouter des intergiciels (middleware) supplémentaire à la branche devant votre intergiciel (middleware) gestionnaire.

Intergiciel ajouté au pipeline avant la branche sera appelée sur toutes les demandes ; la branche n’aura aucun impact sur ces derniers.

## <a name="loading-middleware-options-using-the-options-pattern"></a>Options d’intergiciel (middleware) à l’aide du modèle d’options de chargement

Certains modules et les gestionnaires ont des options de configuration qui sont stockées dans *Web.config*. Toutefois, un nouveau modèle de configuration dans ASP.NET Core est utilisé à la place de *Web.config*.

La nouvelle [système de configuration](xref:fundamentals/configuration/index) vous offre des options suivantes pour résoudre ce problème :

* Injecter directement les options dans l’intergiciel (middleware), comme indiqué dans le [section suivante](#loading-middleware-options-through-direct-injection).

* Utilisez le [modèle options](xref:fundamentals/configuration/options):

1. Créez une classe pour contenir vos options de l’intergiciel (middleware), par exemple :

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. Store les valeurs d’option

   Le système de configuration vous permet de vous permettent de stocker des valeurs d’option n’importe où que vous souhaitez. Toutefois, les sites plus utiliser *appsettings.json*, de sorte que nous allons jeter cette approche :

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection* ici est un nom de section. Il ne doit pas être le même que le nom de votre classe d’options.

3. Associer les valeurs d’option à la classe d’options

    Le modèle d’options utilise l’infrastructure d’injection de dépendance d’ASP.NET Core pour associer le type options (telles que `MyMiddlewareOptions`) avec un `MyMiddlewareOptions` objet qui a les options réelles.

    Mise à jour votre `Startup` classe :

   1. Si vous utilisez *appsettings.json*, ajoutez-le au Générateur de configuration dans le `Startup` constructeur :

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. Configurer le service d’options :

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. Associez vos options à votre classe d’options :

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. Injecter les options dans votre constructeur d’intergiciel (middleware). Cela est similaire à l’injection d’options dans un contrôleur.

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   Le [UseMiddleware](#http-modules-usemiddleware) méthode d’extension qui ajoute votre intergiciel (middleware) pour le `IApplicationBuilder` s’occupe de l’injection de dépendances.

   Ce n’est pas limité à `IOptions` objets. Tout autre objet qui requiert votre intergiciel (middleware) peut être injectée de cette façon.

## <a name="loading-middleware-options-through-direct-injection"></a>Chargement des options d’intergiciel (middleware) via l’injection directe

Le modèle options présente l’avantage qu’il crée un couplage faible entre les valeurs des options et leurs clients. Une fois que vous avez associé une classe d’options avec les valeurs d’options réel, une autre classe peut accéder aux options via l’infrastructure d’injection de dépendance. Il est inutile pour faire circuler les valeurs des options.

Cela répartit cependant si vous souhaitez utiliser l’intergiciel (middleware) même à deux reprises, avec différentes options. Par exemple un intergiciel d’autorisation utilisé dans différentes branches autorisant des rôles différents. Vous ne pouvez pas associer deux objets de différentes options avec la classe d’une options.

La solution consiste à obtenir les objets d’options avec les valeurs d’options réel dans votre `Startup` classe et de transférer ces informations directement à chaque instance de votre intergiciel (middleware).

1. Ajouter une deuxième clé à *appsettings.json*

   Pour ajouter un deuxième ensemble d’options pour le *appsettings.json* , utilisez une nouvelle clé pour l’identifier :

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. Récupérer les valeurs des options et les transmettre à un intergiciel (middleware). Le `Use...` méthode d’extension (ce qui ajoute votre intergiciel (middleware) au pipeline) est un emplacement logique pour passer les valeurs d’option : 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. Activer l’intergiciel (middleware) prendre un paramètre d’options. Fournissez une surcharge de la `Use...` méthode d’extension (qui prend le paramètre options et le passe à `UseMiddleware`). Lorsque `UseMiddleware` est appelée avec des paramètres, il transmet les paramètres à votre constructeur d’intergiciel (middleware) lorsqu’il instancie l’objet de l’intergiciel (middleware).

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   Notez la façon dont cela encapsule l’objet d’options dans un `OptionsWrapper` objet. Cela permet d’implémenter `IOptions`, comme prévu par le constructeur d’intergiciel (middleware).

## <a name="migrating-to-the-new-httpcontext"></a>Migration vers le nouveau HttpContext

Vous avez vu précédemment qui le `Invoke` méthode dans votre intergiciel (middleware) prend un paramètre de type `HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` a considérablement changé dans ASP.NET Core. Cette section montre comment traduire les propriétés couramment utilisées de [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) vers le nouveau `Microsoft.AspNetCore.Http.HttpContext`.

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**ID de requête unique (aucun équivalent System.Web.HttpContext)**

Vous donne un id unique pour chaque demande. Très pratique d’inclure dans vos journaux.

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url** et **HttpContext.Request.RawUrl** traduire en :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> Lire les valeurs de formulaire uniquement si le type de contenu sub est *x--www-form-urlencoded* ou *données de formulaire*.

**HttpContext.Request.InputStream** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> Utilisez ce code uniquement dans un gestionnaire type intergiciel (middleware), à la fin d’un pipeline.
>
>Vous pouvez lire le corps brut comme indiqué ci-dessus une seule fois par demande. Intergiciel (middleware) tente de lire le corps après la première lecture lit un corps vide.
>
>Cela ne s’applique pas à lire un formulaire comme indiqué précédemment, étant donné que cette opération effectuée à partir d’une mémoire tampon.

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status** et **HttpContext.Response.StatusDescription** traduire en :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding** et **HttpContext.Response.ContentType** traduire en :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType** sur son propre également se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output** se traduit par :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

Qui sert un fichier est abordée [ici](../fundamentals/request-features.md#middleware-and-request-features).

**HttpContext.Response.Headers**

Envoi des en-têtes de réponse est compliquée par le fait que si vous leur attribuez une fois que tout a été écrite dans le corps de réponse, elles ne seront pas envoyées.

La solution consiste à définir une méthode de rappel qui sera appelée droite avant d’écrire sur le démarrage de la réponse. Cette opération s’effectue mieux au début de la `Invoke` méthode dans votre intergiciel (middleware). Il s’agit cette méthode de rappel qui définit les en-têtes de réponse.

Le code suivant définit une méthode de rappel appelée `SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Le `SetHeaders` méthode de rappel ressemblerait à ceci :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

Les cookies sont transmis au navigateur dans un *Set-Cookie* en-tête de réponse. Par conséquent, envoi de cookies nécessite le rappel de même que celles utilisées pour l’envoi des en-têtes de réponse :

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

Le `SetCookies` méthode de rappel ressemblerait à ce qui suit :

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Vue d’ensemble des Modules HTTP et les gestionnaires HTTP](/iis/configuration/system.webserver/)
* [Configuration](xref:fundamentals/configuration/index)
* [Démarrage d’une application](xref:fundamentals/startup)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
