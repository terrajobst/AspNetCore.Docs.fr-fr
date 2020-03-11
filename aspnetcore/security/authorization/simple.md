---
title: Autorisation simple dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’attribut Authorize pour restreindre l’accès aux contrôleurs et aux actions de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663582"
---
# <a name="simple-authorization-in-aspnet-core"></a>Autorisation simple dans ASP.NET Core

<a name="security-authorization-simple"></a>

L’autorisation dans MVC est contrôlée par le biais de l’attribut `AuthorizeAttribute` et de ses différents paramètres. Au plus simple, l’application de l’attribut `AuthorizeAttribute` à un contrôleur ou à une action limite l’accès au contrôleur ou à l’action à n’importe quel utilisateur authentifié.

Par exemple, le code suivant limite l’accès à l' `AccountController` à n’importe quel utilisateur authentifié.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Si vous souhaitez appliquer une autorisation à une action plutôt qu’au contrôleur, appliquez l’attribut `AuthorizeAttribute` à l’action elle-même :

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Désormais, seuls les utilisateurs authentifiés peuvent accéder à la fonction `Logout`.

Vous pouvez également utiliser l’attribut `AllowAnonymous` pour permettre à des utilisateurs non authentifiés d’accéder à des actions individuelles. Par exemple :

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Cela permettrait uniquement aux utilisateurs authentifiés d’accéder au `AccountController`, à l’exception de l’action `Login`, qui est accessible par tout le monde, indépendamment de son état authentifié ou non authentifié/anonyme.

> [!WARNING]
> `[AllowAnonymous]` ignore toutes les instructions d’autorisation. Si vous combinez `[AllowAnonymous]` et tout attribut `[Authorize]`, les attributs `[Authorize]` sont ignorés. Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, tous les attributs de `[Authorize]` sur le même contrôleur (ou sur toute action qu’il contient) sont ignorés.
