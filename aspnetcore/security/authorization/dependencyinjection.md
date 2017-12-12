---
title: "Injection de dépendances dans les gestionnaires d’exigence"
author: rick-anderson
description: "Ce document décrit comment injecter des gestionnaires de demande d’autorisation dans une application ASP.NET Core en utilisant l’injection de dépendance."
keywords: "ASP.NET Core, injection de dépendances, le Gestionnaire d’autorisation"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="27532-104">Injection de dépendances dans les gestionnaires d’exigence</span><span class="sxs-lookup"><span data-stu-id="27532-104">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="27532-105">[Les gestionnaires d’autorisation doivent être inscrit](policies.md#handler-registration) dans la collection de service lors de la configuration (à l’aide de [injection de dépendance](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="27532-105">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="27532-106">Supposons que vous disposiez d’un référentiel de règles que vous souhaitez évaluer à l’intérieur d’un gestionnaire d’autorisation, et ce référentiel a été inscrit dans la collection de service.</span><span class="sxs-lookup"><span data-stu-id="27532-106">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="27532-107">L’autorisation sera résoudre et qui injecter dans votre constructeur.</span><span class="sxs-lookup"><span data-stu-id="27532-107">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="27532-108">Par exemple, si vous souhaitez utiliser ASP. NET de journalisation de l’infrastructure que vous ne souhaitez pas injecter `ILoggerFactory` dans votre gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="27532-108">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="27532-109">Un gestionnaire de ce type peut ressembler à :</span><span class="sxs-lookup"><span data-stu-id="27532-109">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="27532-110">Vous devez inscrire le gestionnaire avec `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="27532-110">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="27532-111">Une instance du gestionnaire va être créé au démarrage de votre application et qui seront DI injecter inscrit `ILoggerFactory` dans votre constructeur.</span><span class="sxs-lookup"><span data-stu-id="27532-111">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="27532-112">Gestionnaires d’utilisent Entity Framework ne doit pas être enregistrés comme singletons.</span><span class="sxs-lookup"><span data-stu-id="27532-112">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
