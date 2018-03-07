---
title: Autorisation simple
author: rick-anderson
description: "Ce document explique comment utiliser l’attribut Authorize pour restreindre l’accès aux actions et les contrôleurs ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 503ebc665efd460a85f49844ddc847eb12114308
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/11/2018
---
# <a name="simple-authorization"></a>Autorisation simple

<a name="security-authorization-simple"></a>

Dans MVC Les authorisations sont contrôlées par le biais de l'attribut `AuthorizeAttribute` et de ses paramètres différents. Pour faire simple appliquer l'attribut `AuthorizeAttribute` à un contrôleur ou à une action pour pour limiter l’accès au contrôleur ou à une action à n’importe quel utilisateur authentifié.

Par exemple, le code suivant limite l’accès à la `AccountController` à tout utilisateur authentifié.

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

Si vous souhaitez appliquer une autorisation à une action plutôt que sur le contrôleur, appliquer la `AuthorizeAttribute` d’attribut pour l’action à proprement dite :

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

Maintenant seulement les utilisateurs authentifiés peuvent accéder à la fonction de `Logout`.

Vous pouvez également utiliser le `AllowAnonymous` attribut pour permettre l’accès des utilisateurs non authentifiés à chacune des actions. Exemple :

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

Ainsi, seuls les utilisateurs authentifiés ont accès à `AccountController`, à l’exception de l'action `Login`, qui est accessible par tout le monde, quelle que soit leur état authentifié ou non authentifié / anonyme.

>[!WARNING]
> L'attribut `[AllowAnonymous]` ignore toutes les instructions d’autorisation. Si vous combinez `[AllowAnonymous]` et n’importe quel attribut `[Authorize]`, les attributs `[Authorize]` seront toujours ignorés. Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, tous les attributs `[Authorize]` sur le même contrôleur, ou sur toute actions qu’il contient seront ignorés.
