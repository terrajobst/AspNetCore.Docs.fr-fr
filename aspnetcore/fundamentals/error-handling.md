---
title: Gérer les erreurs dans ASP.NET Core
author: ardalis
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273707"
---
# <a name="handle-errors-in-aspnet-core"></a>Gérer les erreurs dans ASP.NET Core

Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)

Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Page d’exception de développeur

Pour configurer une application afin qu’elle affiche une page qui indique des informations détaillées sur les exceptions, installez le package NuGet `Microsoft.AspNetCore.Diagnostics` et ajoutez une ligne à la [méthode Configure dans la classe Startup](xref:fundamentals/startup) :

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Placez `UseDeveloperExceptionPage` avant tout intergiciel (middleware) dans lequel vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.

>[!WARNING]
> Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**. Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production. [Découvrez en plus sur la configuration d’environnements](xref:fundamentals/environments).

Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application. Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande. Le premier onglet inclut une trace de pile. 

![Trace de pile](error-handling/_static/developer-exception-page.png)

L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant.

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

Cette demande ne contenait pas de cookies, sinon ils apparaîtraient sous l’onglet **Cookies**. Vous pouvez voir les en-têtes qui ont été passés sous le dernier onglet.

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configuration d’une page personnalisée de gestion des exceptions

Configurez une page de gestionnaire d’exceptions à utiliser quand l’application n’est pas en cours d’exécution dans l’environnement `Development`.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Dans une application Razor Pages, le modèle Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) fournit une page Error et une classe de modèle de page `ErrorModel` dans le dossier *Pages*.

Dans une application MVC, ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`. Les verbes explicites empêchent certaines demandes d’atteindre la méthode. Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.

Par exemple, la méthode de gestionnaire d’erreurs suivante est fournie par le modèle MVC [dotnet new](/dotnet/core/tools/dotnet-new) et apparaît dans le contrôleur Home :

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Configuration des pages de codes d’état

Par défaut, une application ne fournit pas de page de codes d’état très complète pour les codes d’état HTTP, comme *404 (introuvable)*. Pour fournir des pages de codes d’état, configurez le middleware de pages de codes d’état en ajoutant une ligne à la méthode `Startup.Configure` :

```csharp
app.UseStatusCodePages();
```

Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires simples de texte uniquement pour les codes d’état courants, comme 404 :

![Page 404](error-handling/_static/default-404-status-code.png)

Le middleware prend en charge plusieurs méthodes d’extension. Une méthode prend une expression lambda :

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Une autre méthode prend un type de contenu et une chaîne de format :

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Il existe également des méthodes d’extension de redirection et de réexécution. La méthode de redirection envoie un code d’état 302 au client :

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

La méthode de réexécution retourne le code d’état d’origine au client, mais exécute également le gestionnaire pour l’URL de redirection :

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Les pages de codes d’état peuvent être désactivées pour des requêtes spécifiques dans une méthode de gestionnaire de Pages Razor ou dans un contrôleur MVC. Pour désactiver des pages de codes d’état, essayez de récupérer le [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) auprès de la collection [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) de la requête et désactivez la fonctionnalité si elle est disponible :

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Si vous utilisez une surcharge `UseStatusCodePages*` qui pointe vers un point de terminaison dans l’application, créez une vue MVC ou Razor Page pour le point de terminaison. Par exemple, le modèle [dotnet new](/dotnet/core/tools/dotnet-new) pour une application Razor Pages génère les page et classe de modèle de page suivantes :

*Error.cshtml* :

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs* :

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Code de gestion des exceptions

Le code dans les pages de gestion des exceptions peut lever des exceptions. Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.

En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter. La réponse doit être accomplie ou la connexion abandonnée.

## <a name="server-exception-handling"></a>Gestion des exceptions de serveur

En plus de la logique de gestion des exceptions dans votre application, le [serveur](xref:fundamentals/servers/index) qui héberge votre application effectue certaines tâches de gestion des exceptions. Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps. Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion. Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur. Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur. Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.

## <a name="startup-exception-handling"></a>Gestion des exceptions de démarrage

Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application. En utilisant l’[hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.

La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port. Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet plante et aucune page d’erreur n’est affichée quand l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).

En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 Échec du processus* est retournée par le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) si le processus ne peut pas être a démarré. Suivez les conseils de dépannage de la rubrique [Résoudre les problèmes liés à ASP.NET Core sur IIS](xref:host-and-deploy/iis/troubleshoot).

## <a name="aspnet-mvc-error-handling"></a>Gestion des erreurs MVC ASP.NET

Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.

### <a name="exception-filters"></a>Filtres d’exception

Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC. Ces filtres sont uniquement appelés pour gérer les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre. Découvrez-en plus sur les filtres d’exception dans [Filtres](xref:mvc/controllers/filters).

> [!TIP]
> Les filtres d’exception sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel (middleware) de gestion des erreurs. En règle générale, préférez l’intergiciel (middleware), et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.

### <a name="handling-model-state-errors"></a>Gestion des erreurs d’état de modèle

La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle ; dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec les états de modèles non valide. Pour plus d’informations, consultez [Tester la logique du contrôleur](xref:mvc/controllers/testing).
