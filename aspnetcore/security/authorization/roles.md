---
title: Autorisation basée sur des rôles dans ASP.NET Core
author: rick-anderson
description: Découvrez comment restreindre l’accès de contrôleur et d’action ASP.NET Core en passant des rôles à l’attribut Authorize.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: f1e7209cae1e2a58ad536547d655dd744ca0d3f7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="role-based-authorization-in-aspnet-core"></a>Autorisation basée sur des rôles dans ASP.NET Core

<a name="security-authorization-role-based"></a>

Lorsqu’une identité est créée, elle peut appartenir à un ou plusieurs rôles. Par exemple, Tracy mon peut appartenir aux rôles administrateur et utilisateur tandis que Scott peut uniquement appartenir au rôle d’utilisateur. Comment ces rôles sont créés et gérés varient selon le magasin de stockage du processus d’autorisation. Les rôles sont exposés au développeur via les [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) méthode sur le [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) classe.

## <a name="adding-role-checks"></a>Ajout de contrôles de rôle

Les vérifications d’autorisation basée sur les rôles sont déclaratives&mdash;le développeur les incorpore dans son code, sur un contrôleur ou une action d'un contrôleur, en spécifiant les rôles auxquels l’utilisateur actuel doit être membre afin d’accéder à la ressource demandée.

Par exemple, le code suivant limite l’accès à toutes les actions sur le `AdministrationController` aux utilisateurs qui sont membres du rôle `Administrator` :

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Vous pouvez spécifier plusieurs rôles à l'aide d'une liste séparée par des virgules :

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Ce contrôleur est accessible uniquement par les utilisateurs qui sont membres du rôle `HRManager` ou du rôle `Finance`.

Si vous appliquez plusieurs attributs, un utilisateur doit être un membre de tous les rôles spécifiés ; l’exemple suivant nécessite qu’un utilisateur soit membre du rôle `PowerUser` et du rôle `ControlPanelUser`.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Vous pouvez également limiter l’accès en appliquant des attributs d’autorisation de rôle supplémentaires au niveau de l’action :

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

Dans l’extrait de code précédent, les membres du rôle `Administrator` ou du rôle `PowerUser` peuvent accéder au contrôleur et à l'action `SetTime`, mais seuls les membres du rôle `Administrator` peuvent accéder à l'action `ShutDown`.

Vous pouvez également verrouiller un contrôleur et autoriser l’accès anonyme, non authentifié à des actions individuelles.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Contrôles de rôle basée sur la stratégie

Les exigences pour les rôles peuvent également être exprimés en utilisant la syntaxe de la nouvelle stratégie, où un développeur inscrit une stratégie au démarrage dans la configuration du service d’autorisation. Ceci se fait normalement dans la méthode `ConfigureServices()` de votre fichier *Startup.cs*.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Les stratégies sont appliquées à l’aide de la propriété `Policy` sur l'attribut `AuthorizeAttribute` :

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Si vous souhaitez spécifier plusieurs rôles autorisés dans une spécification, vous pouvez les spécifier en tant que paramètres à la méthode `RequireRole` :

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

Cet exemple autorise l'accès aux utilisateurs qui appartiennent aux rôles `Administrator`, `PowerUser` ou `BackupAdministrator`.
