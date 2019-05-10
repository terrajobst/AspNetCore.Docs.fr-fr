---
title: Injection de dépendances dans les gestionnaires d’exigences dans ASP.NET Core
author: rick-anderson
description: Découvrez comment injecter des gestionnaires d’exigences d’autorisation dans une application ASP.NET Core à l’aide de l’injection de dépendances.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896366"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Injection de dépendances dans les gestionnaires d’exigences dans ASP.NET Core

<a name="security-authorization-di"></a>

[Les gestionnaires d’autorisation doivent être inscrit](xref:security/authorization/policies#handler-registration) dans la collection de service lors de la configuration (à l’aide de [l’injection de dépendances](xref:fundamentals/dependency-injection)).

Supposons que vous ayez un référentiel de règles que vous souhaitez évaluer à l’intérieur d’un gestionnaire d’autorisation, et ce référentiel a été inscrit dans la collection de service. L’autorisation sera résoudre et qui injecter dans votre constructeur.

Par exemple, si vous souhaitez utiliser ASP. NET d’enregistrement d’infrastructure, vous pouvez injecter `ILoggerFactory` dans votre gestionnaire. Ce gestionnaire peut ressembler :

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

Vous devez enregistrer le gestionnaire avec `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Une instance du gestionnaire va être créé au démarrage de votre application et est de l’injection de dépendances injecter inscrit `ILoggerFactory` dans votre constructeur.

> [!NOTE]
> Les gestionnaires qui utilisent Entity Framework ne doit pas être enregistrés en tant que singletons.
