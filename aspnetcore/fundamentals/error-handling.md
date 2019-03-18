---
title: Gérer les erreurs dans ASP.NET Core
author: tdykstra
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665361"
---
# <a name="handle-errors-in-aspnet-core"></a>Gérer les erreurs dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex) et [Steve Smith](https://ardalis.com/)

Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Page d’exceptions du développeur

Pour configurer une application de sorte qu’elle affiche une page contenant des informations détaillées sur les exceptions des requêtes, utilisez la *page Exception pour les développeurs*. Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Ajoutez une ligne à la méthode `Startup.Configure` lorsque l’application s’exécute dans [l’environnement](xref:fundamentals/environments) de développement :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

Placez l’appel à <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> devant tout middleware où vous souhaitez intercepter des exceptions.

> [!WARNING]
> Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**. Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production. Pour plus d’informations sur la configuration des environnements, consultez <xref:fundamentals/environments>.

Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application. Cette page inclut les informations suivantes sur l’exception et la demande :

* Trace de pile
* Paramètres de la chaîne de requête (le cas échéant)
* Cookies (le cas échéant)
* En-têtes

## <a name="configure-a-custom-exception-handling-page"></a>Configurer une page personnalisée de gestion des exceptions

Lorsque l’application n’est pas exécutée dans l’environnement de développement, appelez la méthode d’extension <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> pour ajouter le middleware de gestion des exceptions. Le middleware :

* Intercepte les exceptions.
* Journalise les exceptions.
* Réexécute la requête dans un autre pipeline pour la page ou le contrôleur indiqué(e). La demande n’est pas réexécutée si la réponse a démarré.

Dans l’exemple suivant à partir de l’exemple d’application, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ajoute le middleware de gestion des exceptions dans des environnements qui ne sont pas de développement. La méthode d’extension spécifie une page d’erreur ou un contrôleur au point de terminaison `/Error` pour les demandes réexécutées une fois que les exceptions sont interceptées et journalisées :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

Le modèle d’application Razor Pages fournit une page d’erreur (*.cshtml*) et une classe <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> (`ErrorModel`) dans le dossier Pages.

Dans une application MVC, la méthode de gestionnaire d’erreurs suivante est incluse dans le modèle d’application MVC et apparaît dans le contrôleur Home :

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

Ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`. Les verbes explicites empêchent certaines demandes d’atteindre la méthode. Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.

## <a name="access-the-exception"></a>Accéder à l'exception

Utilisez <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> pour accéder à l’exception ou au chemin de la requête d’origine dans un contrôleur ou une page :

* Le chemin est accessible avec la propriété <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path>.
* Lisez <xref:System.Exception?displayProperty=fullName> à partir de la propriété [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) héritée.

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> Ne distribuez **pas** d’informations sensibles sur les erreurs de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> ou de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> aux clients. Cela représenterait un risque de sécurité.

## <a name="configure-custom-exception-handling-code"></a>Configurer du code personnalisé de gestion des exceptions

Pour éviter de distribuer les erreurs à un point de terminaison avec une [page personnalisée de gestion des exceptions](#configure-a-custom-exception-handling-page), il est possible de fournir une expression lambda à <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. À l’aide d’une expression lambda avec <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> permet d’accéder à l’erreur avant de renvoyer la réponse.

L’exemple d’application illustre le code personnalisé de gestion des exceptions dans `Startup.Configure`. Déclenchez une exception avec le lien **Lever une exception** sur la page d’index. L’expression lambda suivante s’exécute :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> Ne distribuez **pas** d’informations sensibles sur les erreurs de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> ou de <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> aux clients. Cela représenterait un risque de sécurité.

## <a name="configure-status-code-pages"></a>Configurer des pages de codes d’état

Par défaut, une application ASP.NET Core ne fournit pas une page de codes d’état pour les codes d’état HTTP, comme *404 - Introuvable*. L’application retourne un code d’état et un corps de réponse vide. Pour fournir des pages de codes d’état, utilisez le middleware (intergiciel) de pages de codes d’état.

Le middleware est mis à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Ajoutez une ligne à la méthode `Startup.Configure` :

```csharp
app.UseStatusCodePages();
```

Appelez la méthode <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> avant les middlewares de gestion des requêtes (comme le middleware de fichiers statiques et le middleware MVC).

Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires de texte uniquement pour les codes d’état courants, comme *404 - Introuvable* :

```
Status Code: 404; Not Found
```

Le middleware prend en charge plusieurs méthodes d’extension qui vous permettent de personnaliser son comportement.

Une surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prend une expression lambda, que vous pouvez utiliser pour traiter la logique de gestion des erreurs personnalisée et écrire manuellement la réponse :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Une surcharge de <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> prend un type de contenu et une chaîne de format, que vous pouvez utiliser pour personnaliser le type de contenu et le texte de la réponse :

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Rediriger et réexécuter les méthodes d’extension

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Envoie un code d’état *302 - Trouvé* au client.
* Redirige le client à l’emplacement fourni dans le modèle d’URL.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> est généralement utilisée lorsque l’application :

* Doit rediriger le client vers un autre point de terminaison, généralement dans les cas où une autre application traite l’erreur. Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison redirigé.
* Ne doit pas conserver ni retourner le code d’état d’origine avec la réponse de redirection initiale.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Retourne le code d’état d’origine au client.
* Génère le corps de la réponse en réexécutant le pipeline de requête avec un autre chemin.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> est généralement utilisée lorsque l’application doit :

* Traiter la demande sans la rediriger vers un autre point de terminaison. Pour les applications web, la barre d’adresses du navigateur client reflète le point de terminaison demandé à l’origine.
* Conserver et retourner le code d’état d’origine avec la réponse.

Les modèles peuvent inclure un espace réservé (`{0}`) pour le code d’état. Le modèle doit commencer par une barre oblique (`/`). Lorsque vous utilisez un espace réservé, vérifiez que le point de terminaison (page ou contrôleur) peut traiter le segment de chemin d’accès. Par exemple, une Razor Page pour les erreurs doit accepter la valeur du segment de chemin d’accès facultatif avec la directive `@page` :

```cshtml
@page "{code?}"
```

Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC. Pour désactiver des pages de codes d’état, essayez de récupérer le <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> auprès de la collection [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) de la requête et désactivez la fonctionnalité si elle est disponible :

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Pour utiliser une surcharge <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> qui pointe vers un point de terminaison dans l’application, créez une vue MVC ou Razor Page pour le point de terminaison. Par exemple, le modèle d’application Razor Pages génère les page et classe de modèle de page suivantes :

*Error.cshtml* :

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs* :

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs* :

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>Code de gestion des exceptions

Le code dans les pages de gestion des exceptions peut lever des exceptions. Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.

En outre, n’oubliez pas qu’une fois que les en-têtes de réponse sont envoyés :

* L’application ne peut pas modifier le code d’état de la réponse.
* Il est impossible d’exécuter les pages d’exception ou les gestionnaires. La réponse doit être accomplie ou la connexion abandonnée.

## <a name="server-exception-handling"></a>Gestion des exceptions de serveur

En plus de la logique de gestion des exceptions dans votre application, [l’implémentation de serveur](xref:fundamentals/servers/index) peut gérer certaines exceptions. Si le serveur intercepte une exception avant que les en-têtes de réponse ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps de réponse. Si le serveur intercepte une exception une fois que les en-têtes de réponse ont été envoyés, il ferme la connexion. Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur. Toute exception qui se produit tandis que le serveur traite la demande est gérée par le dispositif de gestion des exceptions du serveur. Ni les pages d’erreur personnalisées de l’application, ni les intergiciels (middleware) de gestion des exceptions, ni les filtres n’ont d’incidence sur ce comportement.

## <a name="startup-exception-handling"></a>Gestion des exceptions de démarrage

Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application. En utilisant [l’hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.

La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port. Si une liaison échoue pour une raison quelconque :

* La couche d’hébergement journalise une exception critique.
* Le processus dotnet tombe en panne.
* Aucune page d’erreur ne s’affiche lorsque l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).

En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 - Échec du processus* est retournée par le [module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) si le processus ne peut pas démarrer. Pour plus d'informations, consultez <xref:host-and-deploy/iis/troubleshoot>. Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec Azure App Service, consultez le site <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Gestion des erreurs MVC ASP.NET MVC

Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.

### <a name="exception-filters"></a>Filtres d’exceptions

Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC. Ces filtres gèrent les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre. Ils ne sont appelés dans aucun autre cas. Pour plus d'informations, consultez <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Les filtres d’exceptions sont utiles pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que le middleware de gestion des exceptions. Nous vous recommandons d’utiliser le middleware. Utilisez uniquement des filtres si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.

### <a name="handle-model-state-errors"></a>Gérer les erreurs d’état de modèle

La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) et de réagir de façon appropriée.

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de[ validation de modèle](xref:mvc/models/validation). Dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec les états de modèles non valide. Pour plus d'informations, consultez <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
