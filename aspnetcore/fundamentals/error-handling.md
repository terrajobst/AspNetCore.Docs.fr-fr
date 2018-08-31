---
title: Gérer les erreurs dans ASP.NET Core
author: ardalis
description: Découvrez comment gérer les erreurs dans les applications ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: ff04ebeb6a682ec924afe896fd6716010a63f7cd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751612"
---
# <a name="handle-errors-in-aspnet-core"></a>Gérer les erreurs dans ASP.NET Core

Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)

Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Page d’exception de développeur

::: moniker range=">= aspnetcore-2.1"

Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*. Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Ajoutez une ligne à la méthode `Startup.Configure` :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*. Cette page est mise à disposition par le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/), qui est disponible dans le [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Ajoutez une ligne à la méthode `Startup.Configure` :

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour configurer une application pour afficher une page qui fournit des informations détaillées sur les exceptions, utilisez la *page d’exception de développeur*. Cette page est accessible en ajoutant une référence de package pour le package [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) dans le fichier projet. Ajoutez une ligne à la méthode `Startup.Configure` :

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Placez l’appel à [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) devant tout intergiciel (middleware) où vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.

>[!WARNING]
> Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**. Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production. [Découvrez en plus sur la configuration d’environnements](xref:fundamentals/environments).

Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application. Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande. Le premier onglet inclut une trace de pile :

![Trace de pile](error-handling/_static/developer-exception-page.png)

L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant :

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

Si la requête a des cookies, ceux-ci apparaissent sous l’onglet **Cookies**. Les en-têtes sont visibles sous le dernier onglet :

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configuration d’une page personnalisée de gestion des exceptions

Configurez une page de gestionnaire d’exceptions à utiliser quand l’application ne s’exécute pas dans l’environnement `Development` :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Dans une application Razor Pages, le modèle Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) fournit une page Error et une classe de modèle de page `ErrorModel` dans le dossier *Pages*.

Dans une application MVC, ne décorez pas la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`. Les verbes explicites empêchent certaines demandes d’atteindre la méthode. Autorisez l’accès anonyme à la méthode afin que les utilisateurs non authentifiés puissent recevoir la vue des erreurs.

Par exemple, la méthode de gestionnaire d’erreurs suivante est fournie par le modèle MVC [dotnet new](/dotnet/core/tools/dotnet-new) et apparaît dans le contrôleur Home :

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Configuration des pages de codes d’état

Par défaut, une application ne fournit pas de page de codes d’état très complète pour les codes d’état HTTP, comme *404 (introuvable)*. Pour fournir des pages de codes d’état, configurez le middleware de pages de codes d’état en ajoutant une ligne à la méthode `Startup.Configure` :

```csharp
app.UseStatusCodePages();
```

Par défaut, le middleware de pages de codes d’état ajoute des gestionnaires de texte uniquement pour les codes d’état courants, comme 404 :

![Page 404](error-handling/_static/default-404-status-code.png)

Le middleware prend en charge plusieurs méthodes d’extension. Une méthode prend une expression lambda :

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Une autre méthode prend un type de contenu et une chaîne de format :

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Il existe également des méthodes d’extension de redirection et de réexécution. La méthode de redirection envoie un code d’état *302 Trouvé* au client et redirige ce dernier vers le modèle d’URL d’emplacement fourni. Le modèle peut inclure un espace réservé `{0}` pour le code d’état. Le chemin de base est ajouté aux URL commençant par `~`. Une URL qui ne commence pas par `~` est utilisée en l’état.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

La méthode de réexécution retourne le code d’état d’origine au client et indique que le corps de la réponse doit être généré en réexécutant le pipeline de requête avec un autre chemin. Ce chemin peut contenir un espace réservé `{0}` pour le code d’état :

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

## <a name="exception-handling-code"></a>Code de gestion des exceptions

Le code dans les pages de gestion des exceptions peut lever des exceptions. Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.

En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter. La réponse doit être accomplie ou la connexion abandonnée.

## <a name="server-exception-handling"></a>Gestion des exceptions de serveur

En plus de la logique de gestion des exceptions dans votre application, le [serveur](xref:fundamentals/servers/index) qui héberge votre application effectue certaines tâches de gestion des exceptions. Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse *500 Erreur interne du serveur* sans corps. Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion. Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur. Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur. Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.

## <a name="startup-exception-handling"></a>Gestion des exceptions de démarrage

Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application. En utilisant l’[hôte web](xref:fundamentals/host/web-host), vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au démarrage](xref:fundamentals/host/web-host#detailed-errors) à l’aide des clés `captureStartupErrors` et `detailedErrors`.

La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port. Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet plante et aucune page d’erreur n’est affichée quand l’application s’exécute sur le serveur [Kestrel](xref:fundamentals/servers/kestrel).

En cas d’exécution sur [IIS](/iis) ou [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), une réponse *502.5 Échec du processus* est retournée par le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) si le processus ne peut pas être a démarré. Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec IIS, consultez le site <xref:host-and-deploy/iis/troubleshoot>. Pour plus d’informations sur la résolution des problèmes de démarrage lors de l’hébergement avec Azure App Service, consultez le site <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Gestion des erreurs MVC ASP.NET MVC

Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.

### <a name="exception-filters"></a>Filtres d’exceptions

Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC. Ces filtres gèrent les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre. Ils ne sont appelés dans aucun autre cas. Pour en savoir plus, consultez [Filtres](xref:mvc/controllers/filters).

> [!TIP]
> Les filtres d’exceptions sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel de gestion des erreurs. En règle générale, privilégiez l’utilisation de middleware et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.

### <a name="handling-model-state-errors"></a>Gestion des erreurs d’état de modèle

La [validation de modèle](xref:mvc/models/validation) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle. Dans ce cas, un [filtre](xref:mvc/controllers/filters) peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec les états de modèles non valide. Pour plus d’informations, consultez [Tester la logique du contrôleur](xref:mvc/controllers/testing).

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
