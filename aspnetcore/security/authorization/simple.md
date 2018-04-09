---
title: Autorisation simple dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’attribut Authorize pour restreindre l’accès aux actions et les contrôleurs ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: cef5cb146c6c1ff052430748a9a64c6a822d6fa3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="d2f60-103">Autorisation simple dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2f60-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="d2f60-104">Dans MVC Les authorisations sont contrôlées par le biais de l'attribut `AuthorizeAttribute` et de ses paramètres différents.</span><span class="sxs-lookup"><span data-stu-id="d2f60-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="d2f60-105">Pour faire simple appliquer l'attribut `AuthorizeAttribute` à un contrôleur ou à une action pour pour limiter l’accès au contrôleur ou à une action à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="d2f60-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="d2f60-106">Par exemple, le code suivant limite l’accès à la `AccountController` à tout utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="d2f60-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="d2f60-107">Si vous souhaitez appliquer une autorisation à une action plutôt que sur le contrôleur, appliquer la `AuthorizeAttribute` d’attribut pour l’action à proprement dite :</span><span class="sxs-lookup"><span data-stu-id="d2f60-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="d2f60-108">Maintenant seulement les utilisateurs authentifiés peuvent accéder à la fonction de `Logout`.</span><span class="sxs-lookup"><span data-stu-id="d2f60-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="d2f60-109">Vous pouvez également utiliser l'attribut `AllowAnonymous` pour permettre l’accès à des utilisateurs non authentifiés à chacune des actions.</span><span class="sxs-lookup"><span data-stu-id="d2f60-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="d2f60-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d2f60-110">For example:</span></span>

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

<span data-ttu-id="d2f60-111">Ainsi, seuls les utilisateurs authentifiés ont accès à `AccountController`, à l’exception de l'action `Login`, qui est accessible par tout le monde, quelle que soit leur état authentifié ou non authentifié / anonyme.</span><span class="sxs-lookup"><span data-stu-id="d2f60-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="d2f60-112">L'attribut `[AllowAnonymous]` ignore toutes les instructions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d2f60-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="d2f60-113">Si vous combinez `[AllowAnonymous]` et n’importe quel attribut `[Authorize]`, les attributs `[Authorize]` seront toujours ignorés.</span><span class="sxs-lookup"><span data-stu-id="d2f60-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="d2f60-114">Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur, tous les attributs `[Authorize]` sur le même contrôleur, ou sur toute actions qu’il contient seront ignorés.</span><span class="sxs-lookup"><span data-stu-id="d2f60-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
