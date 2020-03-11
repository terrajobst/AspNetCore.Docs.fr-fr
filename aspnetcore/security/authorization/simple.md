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
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="f2f57-103">Autorisation simple dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2f57-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="f2f57-104">L’autorisation dans MVC est contrôlée par le biais de l’attribut `AuthorizeAttribute` et de ses différents paramètres.</span><span class="sxs-lookup"><span data-stu-id="f2f57-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="f2f57-105">Au plus simple, l’application de l’attribut `AuthorizeAttribute` à un contrôleur ou à une action limite l’accès au contrôleur ou à l’action à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="f2f57-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="f2f57-106">Par exemple, le code suivant limite l’accès à l' `AccountController` à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="f2f57-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="f2f57-107">Si vous souhaitez appliquer une autorisation à une action plutôt qu’au contrôleur, appliquez l’attribut `AuthorizeAttribute` à l’action elle-même :</span><span class="sxs-lookup"><span data-stu-id="f2f57-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="f2f57-108">Désormais, seuls les utilisateurs authentifiés peuvent accéder à la fonction `Logout`.</span><span class="sxs-lookup"><span data-stu-id="f2f57-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="f2f57-109">Vous pouvez également utiliser l’attribut `AllowAnonymous` pour permettre à des utilisateurs non authentifiés d’accéder à des actions individuelles.</span><span class="sxs-lookup"><span data-stu-id="f2f57-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="f2f57-110">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f2f57-110">For example:</span></span>

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

<span data-ttu-id="f2f57-111">Cela permettrait uniquement aux utilisateurs authentifiés d’accéder au `AccountController`, à l’exception de l’action `Login`, qui est accessible par tout le monde, indépendamment de son état authentifié ou non authentifié/anonyme.</span><span class="sxs-lookup"><span data-stu-id="f2f57-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="f2f57-112">`[AllowAnonymous]` ignore toutes les instructions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="f2f57-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="f2f57-113">Si vous combinez `[AllowAnonymous]` et tout attribut `[Authorize]`, les attributs `[Authorize]` sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="f2f57-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="f2f57-114">Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, tous les attributs de `[Authorize]` sur le même contrôleur (ou sur toute action qu’il contient) sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="f2f57-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
