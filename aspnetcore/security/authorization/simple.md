---
title: Autorisation simple dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’attribut Authorize pour restreindre l’accès aux actions et les contrôleurs ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961121"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="07e87-103">Autorisation simple dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07e87-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="07e87-104">Dans MVC Les authorisations sont contrôlées par le biais de l'attribut `AuthorizeAttribute` et de ses paramètres différents.</span><span class="sxs-lookup"><span data-stu-id="07e87-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="07e87-105">Pour faire simple appliquer l'attribut `AuthorizeAttribute` à un contrôleur ou à une action pour pour limiter l’accès au contrôleur ou à une action à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="07e87-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="07e87-106">Par exemple, le code suivant limite l’accès à la `AccountController` à tout utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="07e87-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="07e87-107">Si vous souhaitez appliquer une autorisation à une action plutôt que sur le contrôleur, appliquer la `AuthorizeAttribute` d’attribut pour l’action à proprement dite :</span><span class="sxs-lookup"><span data-stu-id="07e87-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="07e87-108">Maintenant seulement les utilisateurs authentifiés peuvent accéder à la fonction de `Logout`.</span><span class="sxs-lookup"><span data-stu-id="07e87-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="07e87-109">Vous pouvez également utiliser l'attribut `AllowAnonymous` pour permettre l’accès à des utilisateurs non authentifiés à chacune des actions.</span><span class="sxs-lookup"><span data-stu-id="07e87-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="07e87-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="07e87-110">For example:</span></span>

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

<span data-ttu-id="07e87-111">Ainsi, seuls les utilisateurs authentifiés ont accès à `AccountController`, à l’exception de l'action `Login`, qui est accessible par tout le monde, quelle que soit leur état authentifié ou non authentifié / anonyme.</span><span class="sxs-lookup"><span data-stu-id="07e87-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="07e87-112">L'attribut `[AllowAnonymous]` ignore toutes les instructions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="07e87-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="07e87-113">Si vous combinez `[AllowAnonymous]` et n’importe quel `[Authorize]` attribut, le `[Authorize]` attributs sont ignorés.</span><span class="sxs-lookup"><span data-stu-id="07e87-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="07e87-114">Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, n’importe quel `[Authorize]` attributs sur le même contrôleur (ou sur toute action qu’il contient) est ignoré.</span><span class="sxs-lookup"><span data-stu-id="07e87-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
