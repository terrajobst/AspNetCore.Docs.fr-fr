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
ms.openlocfilehash: 3299a8fcbd8d8e089d8d7f95e46551c102bcc054
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="8aa8f-103">Autorisation simple</span><span class="sxs-lookup"><span data-stu-id="8aa8f-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="8aa8f-104">Dans MVC est contrôlée par le biais du `AuthorizeAttribute` attribut et ses paramètres différents.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="8aa8f-105">Dans le bloc-notes, appliquer la `AuthorizeAttribute` d’attribut à une limite l’accès contrôleur ou d’action au contrôleur ou une action à n’importe quel utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="8aa8f-106">Par exemple, le code suivant limite l’accès à la `AccountController` à tout utilisateur authentifié.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="8aa8f-107">Si vous souhaitez appliquer l’autorisation à une action plutôt que le contrôleur, appliquer la `AuthorizeAttribute` d’attribut pour l’action proprement dite :</span><span class="sxs-lookup"><span data-stu-id="8aa8f-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="8aa8f-108">Maintenant seulement les utilisateurs authentifiés peuvent accéder le `Logout` (fonction).</span><span class="sxs-lookup"><span data-stu-id="8aa8f-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="8aa8f-109">Vous pouvez également utiliser le `AllowAnonymousAttribute` attribut pour permettre l’accès des utilisateurs non authentifiés à chacune des actions.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="8aa8f-110">Exemple :</span><span class="sxs-lookup"><span data-stu-id="8aa8f-110">For example:</span></span>

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

<span data-ttu-id="8aa8f-111">Ainsi, seuls les utilisateurs authentifiés pour le `AccountController`, à l’exception de la `Login` action, qui est accessible par tout le monde, quelle que soit leur état authentifié ou non authentifié / anonyme.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="8aa8f-112">`[AllowAnonymous]`ignore toutes les instructions d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="8aa8f-113">Si vous appliquez combiner `[AllowAnonymous]` et n’importe quel `[Authorize]` attribut puis les attributs Authorize seront toujours ignorés.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="8aa8f-114">Par exemple, si vous appliquez `[AllowAnonymous]` au niveau du contrôleur de niveau les `[Authorize]` attributs sur le même contrôleur, ou sur toute action qu’il contient seront ignorés.</span><span class="sxs-lookup"><span data-stu-id="8aa8f-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
