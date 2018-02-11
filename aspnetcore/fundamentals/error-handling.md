---
title: Gestion des erreurs dans ASP.NET Core
author: ardalis
description: "Découvrez comment gérer les erreurs dans les applications ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Présentation de la gestion des erreurs dans ASP.NET Core

Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra/)

Cet article aborde des approches courantes pour gérer les erreurs dans les applications ASP.NET Core.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Page d’exception de développeur

Pour configurer une application afin qu’elle affiche une page qui indique des informations détaillées sur les exceptions, installez le package NuGet `Microsoft.AspNetCore.Diagnostics` et ajoutez une ligne à la [méthode Configure dans la classe Startup](startup.md) :

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Placez `UseDeveloperExceptionPage` avant tout intergiciel (middleware) dans lequel vous souhaitez intercepter des exceptions, tel que `app.UseMvc`.

>[!WARNING]
> Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**. Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production. [Découvrez en plus sur la configuration d’environnements](environments.md).

Pour afficher la page d’exception de développeur, exécutez l’exemple d’application avec l’environnement défini sur `Development` et ajoutez `?throw=true` à l’URL de base de l’application. Cette page inclut plusieurs onglets avec des informations sur l’exception et la demande. Le premier onglet inclut une trace de pile. 

![Trace de pile](error-handling/_static/developer-exception-page.png)

L’onglet suivant montre les paramètres de la chaîne de requête, le cas échéant.

![Paramètres de la chaîne de requête](error-handling/_static/developer-exception-page-query.png)

Cette demande ne contenait pas de cookies, sinon ils apparaîtraient sous l’onglet **Cookies**. Vous pouvez voir les en-têtes qui ont été passés sous le dernier onglet.

![En-têtes](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Configuration d’une page de gestion des exceptions personnalisée

Il est judicieux de configurer une page de gestionnaire d’exceptions à utiliser quand l’application n’est pas en cours d’exécution dans l’environnement `Development`.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Dans une application MVC, ne décorez pas explicitement la méthode d’action du gestionnaire d’erreurs avec des attributs de méthode HTTP, tels que `HttpGet`. Utiliser des verbes explicites peut empêcher certaines demandes d’atteindre la méthode.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Configuration des pages de codes d’état

Par défaut, votre application ne fournit pas une page de codes d’état complète pour les codes d’état HTTP tels que 500 (erreur interne du serveur) ou 404 (introuvable). Vous pouvez configurer le `StatusCodePagesMiddleware` en ajoutant une ligne à la méthode `Configure` :

```csharp
app.UseStatusCodePages();
```

Par défaut, cet intergiciel (middleware) ajoute des gestionnaires simples de texte uniquement pour les codes d’état courants, tels que 404 :

![Page 404](error-handling/_static/default-404-status-code.png)

L’intergiciel (middleware) prend en charge plusieurs méthodes d’extension différentes. L’une prend une expression lambda, une autre une chaîne de format et un type de contenu.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Il existe également des méthodes d’extension de redirection. L’une d’elles envoie un code d’état 302 au client, tandis qu’une autre retourne le code d’état d’origine au client, mais exécute également le gestionnaire pour l’URL de redirection.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Si vous devez désactiver les pages de codes d’état pour certaines demandes, vous pouvez le faire :

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Code de gestion des exceptions

Le code dans les pages de gestion des exceptions peut lever des exceptions. Il est souvent judicieux de mettre dans les pages d’erreur de production du contenu purement statique.

En outre, sachez qu’une fois que les en-têtes d’une réponse ont été envoyés, vous ne pouvez pas changer le code d’état de la réponse, et aucun gestionnaire ou page d’exception ne peut s’exécuter. La réponse doit être accomplie ou la connexion abandonnée.

## <a name="server-exception-handling"></a>Gestion des exceptions de serveur

En plus de la logique de gestion des exceptions dans votre application, le [serveur](servers/index.md) qui héberge votre application effectue certaines tâches de gestion des exceptions. Si le serveur intercepte une exception avant que les en-têtes ne soient envoyés, le serveur envoie une réponse d’erreur de serveur interne 500 sans corps. Si le serveur intercepte une exception une fois que les en-têtes ont été envoyés, il ferme la connexion. Les demandes qui ne sont pas gérées par votre application sont gérées par le serveur. Toute exception qui se produit est gérée par le dispositif de gestion des exceptions du serveur. Aucune page d’erreur personnalisée configurée, ni aucun filtre ou intergiciel (middleware) de gestion des exceptions, n’affecte ce comportement.

## <a name="startup-exception-handling"></a>Gestion des exceptions de démarrage

Seule la couche d’hébergement peut gérer les exceptions qui se produisent au démarrage de l’application. Vous pouvez [configurer le comportement de l’hôte en réponse aux erreurs au moment du démarrage](hosting.md#detailed-errors) à l’aide de `captureStartupErrors` et de la clé `detailedErrors`.

La couche d’hébergement ne peut afficher une page d’erreur pour une erreur de démarrage capturée que si celle-ci se produit une fois établie la liaison entre l’adresse de l’hôte et le port. Si une liaison échoue pour une raison quelconque, la couche d’hébergement journalise une exception critique, le processus dotnet échoue et aucune page d’erreur ne s’affiche.

## <a name="aspnet-mvc-error-handling"></a>Gestion des erreurs MVC ASP.NET

Les applications [MVC](xref:mvc/overview) ont des options supplémentaires pour la gestion des erreurs, telles que la configuration de filtres d’exception et l’exécution d’une validation de modèle.

### <a name="exception-filters"></a>Filtres d’exception

Vous pouvez configurer les filtres d’exception globalement ou en fonction d’un contrôleur ou d’une action dans une application MVC. Ces filtres sont uniquement appelés pour gérer les exceptions non prises en charge qui se produisent pendant l’exécution d’une action de contrôleur ou d’un autre filtre. Découvrez-en plus sur les filtres d’exception dans [Filtres](../mvc/controllers/filters.md).

>[!TIP]
> Les filtres d’exception sont adaptés pour intercepter les exceptions qui se produisent dans les actions MVC, mais n’offrent pas la même souplesse que l’intergiciel (middleware) de gestion des erreurs. En règle générale, préférez l’intergiciel (middleware), et n’utilisez des filtres que si vous devez gérer les erreurs *différemment* en fonction de l’action MVC choisie.

### <a name="handling-model-state-errors"></a>Gestion des erreurs d’état de modèle

La [validation de modèle](../mvc/models/validation.md) se produit avant l’appel de chaque action du contrôleur ; il appartient à la méthode d’action d’inspecter `ModelState.IsValid` et de réagir de façon appropriée.

Certaines applications choisissent de suivre une convention standard pour traiter les erreurs de validation de modèle ; dans ce cas, un [filtre](../mvc/controllers/filters.md) peut être un emplacement approprié pour implémenter une telle stratégie. Vous devez tester le comportement de vos actions avec les états de modèles non valide. Pour en savoir plus, consultez [Tester la logique du contrôleur](../mvc/controllers/testing.md).



