---
title: Injection de dépendances dans les gestionnaires d’exigence dans ASP.NET Core
author: rick-anderson
description: Découvrez comment injecter des gestionnaires de demande d’autorisation dans une application ASP.NET Core en utilisant l’injection de dépendance.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273720"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Injection de dépendances dans les gestionnaires d’exigence dans ASP.NET Core

<a name="security-authorization-di"></a>

[Les gestionnaires d’autorisation doivent être inscrit](xref:security/authorization/policies#handler-registration) dans la collection de service lors de la configuration (à l’aide de [injection de dépendance](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

Supposons que vous disposiez d’un référentiel de règles que vous souhaitez évaluer à l’intérieur d’un gestionnaire d’autorisation, et ce référentiel a été inscrit dans la collection de service. L’autorisation sera résoudre et qui injecter dans votre constructeur.

Par exemple, si vous souhaitez utiliser ASP. NET de journalisation de l’infrastructure que vous ne souhaitez pas injecter `ILoggerFactory` dans votre gestionnaire. Un gestionnaire de ce type peut ressembler à :

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

Vous devez inscrire le gestionnaire avec `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Une instance du gestionnaire va être créé au démarrage de votre application et qui seront DI injecter inscrit `ILoggerFactory` dans votre constructeur.

> [!NOTE]
> Gestionnaires d’utilisent Entity Framework ne doit pas être enregistrés comme singletons.
