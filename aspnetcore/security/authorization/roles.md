---
title: "Autorisation basée sur les rôles"
author: rick-anderson
description: "Ce document montre comment restreindre l’accès de contrôleur et d’action ASP.NET Core en passant des rôles à l’attribut Authorize."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: 764d1fcc384fc8370d1a536f9609333de6bd4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="role-based-authorization"></a><span data-ttu-id="ce9af-103">Autorisation basée sur les rôles</span><span class="sxs-lookup"><span data-stu-id="ce9af-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="ce9af-104">Lorsqu’une identité est créée, elle peut appartenir à un ou plusieurs rôles.</span><span class="sxs-lookup"><span data-stu-id="ce9af-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="ce9af-105">Par exemple, Tracy mon peut appartenir aux rôles administrateur et utilisateur tandis que Scott peut uniquement appartenir au rôle d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ce9af-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="ce9af-106">Comment ces rôles sont créés et gérés varient selon le magasin de stockage du processus d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="ce9af-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="ce9af-107">Les rôles sont exposés au développeur via les [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) méthode sur le [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) classe.</span><span class="sxs-lookup"><span data-stu-id="ce9af-107">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="ce9af-108">Ajout de contrôles de rôle</span><span class="sxs-lookup"><span data-stu-id="ce9af-108">Adding role checks</span></span>

<span data-ttu-id="ce9af-109">Les vérifications d’autorisation basée sur les rôles sont déclaratives&mdash;le développeur les incorpore dans leur code, par rapport à un contrôleur ou une action dans un contrôleur, en spécifiant des rôles auxquels l’utilisateur actuel doit être un membre d’accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="ce9af-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="ce9af-110">Par exemple, le code suivant limite l’accès à toutes les actions sur le `AdministrationController` aux utilisateurs qui sont membres de la `Administrator` rôle :</span><span class="sxs-lookup"><span data-stu-id="ce9af-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="ce9af-111">Vous pouvez spécifier plusieurs rôles comme une liste séparée par des virgules :</span><span class="sxs-lookup"><span data-stu-id="ce9af-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="ce9af-112">Ce contrôleur étaient accessibles uniquement par les utilisateurs qui sont membres de la `HRManager` rôle ou le `Finance` rôle.</span><span class="sxs-lookup"><span data-stu-id="ce9af-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="ce9af-113">Si vous appliquez plusieurs attributs, un utilisateur doit être un membre de tous les rôles spécifiés ; l’exemple suivant nécessite qu’un utilisateur doit être membre des groupes le `PowerUser` et `ControlPanelUser` rôle.</span><span class="sxs-lookup"><span data-stu-id="ce9af-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="ce9af-114">Vous pouvez également limiter l’accès en appliquant des attributs d’autorisation de rôle supplémentaires au niveau de l’action :</span><span class="sxs-lookup"><span data-stu-id="ce9af-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="ce9af-115">Dans les membres d’extrait de code précédent de la `Administrator` rôle ou le `PowerUser` rôle peut accéder au contrôleur et le `SetTime` action, mais seuls les membres du `Administrator` rôle peut accéder le `ShutDown` action.</span><span class="sxs-lookup"><span data-stu-id="ce9af-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="ce9af-116">Vous pouvez également verrouiller un contrôleur et autoriser l’accès anonyme, non authentifié à des actions individuelles.</span><span class="sxs-lookup"><span data-stu-id="ce9af-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="ce9af-117">Contrôles de rôle basée sur la stratégie</span><span class="sxs-lookup"><span data-stu-id="ce9af-117">Policy based role checks</span></span>

<span data-ttu-id="ce9af-118">Conditions de rôle peuvent également être exprimées à l’aide de la syntaxe de la nouvelle stratégie, où un développeur inscrit une stratégie au démarrage dans le cadre de la configuration du service d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="ce9af-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="ce9af-119">Ceci se produit normalement dans `ConfigureServices()` dans votre *Startup.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="ce9af-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="ce9af-120">Les stratégies sont appliquées à l’aide de la `Policy` propriété sur le `AuthorizeAttribute` attribut :</span><span class="sxs-lookup"><span data-stu-id="ce9af-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="ce9af-121">Si vous souhaitez spécifier plusieurs rôles autorisés dans une spécification, vous pouvez les spécifier en tant que paramètres à la `RequireRole` méthode :</span><span class="sxs-lookup"><span data-stu-id="ce9af-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="ce9af-122">Cet exemple autorise les utilisateurs qui appartiennent à la `Administrator`, `PowerUser` ou `BackupAdministrator` rôles.</span><span class="sxs-lookup"><span data-stu-id="ce9af-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
