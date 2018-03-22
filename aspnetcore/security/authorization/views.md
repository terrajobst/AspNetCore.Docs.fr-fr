---
title: "Autorisation basée sur le mode dans ASP.NET MVC de base"
author: rick-anderson
description: "Ce document montre comment injecter et utiliser le service d’autorisation à l’intérieur d’une vue ASP.NET Core Razor."
manager: wpickett
ms.author: riande
ms.date: 10/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/views
ms.openlocfilehash: 22754d07882cd704309a4e1a28ad0bf6f69432ea
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="view-based-authorization"></a><span data-ttu-id="bd0de-103">Autorisation basée sur une vue</span><span class="sxs-lookup"><span data-stu-id="bd0de-103">View-based authorization</span></span>

<span data-ttu-id="bd0de-104">Un développeur souhaite souvent afficher, masquer ou modifier une interface utilisateur basée sur l’identité de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="bd0de-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="bd0de-105">Vous pouvez accéder au service de l’autorisation au sein de vues MVC via [l'injection de dépendance](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bd0de-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="bd0de-106">Pour insérer le service d’autorisation dans une vue Razor, utilisez la directive `@inject`:</span><span class="sxs-lookup"><span data-stu-id="bd0de-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="bd0de-107">Si vous souhaitez que le service d’autorisation soit effectif dans chaque vue, placez la directive `@inject` dans le fichier *_ViewImports.cshtml* de la *vues* active.</span><span class="sxs-lookup"><span data-stu-id="bd0de-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="bd0de-108">Pour plus d’informations, consultez [injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bd0de-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="bd0de-109">Utilisez le service d'injection d’autorisation pour appeller `AuthorizeAsync` de la même façon et ainsi vous vérifieriez [les autorisations basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="bd0de-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd0de-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd0de-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd0de-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd0de-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="bd0de-112">Dans certains cas, la ressource sera votre modèle de Vue.</span><span class="sxs-lookup"><span data-stu-id="bd0de-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="bd0de-113">Utilisez `AuthorizeAsync` de la même façon, ainsi vous vérifieriez [les autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="bd0de-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd0de-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd0de-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd0de-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd0de-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="bd0de-116">Dans le code précédent, le modèle est transmis en tant que ressource que l’évaluation de stratégie doit prendre en considération.</span><span class="sxs-lookup"><span data-stu-id="bd0de-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="bd0de-117">Ne vous fiez pas bascule de visibilité des éléments d’interface utilisateur de votre application en tant que le contrôle d’autorisation unique.</span><span class="sxs-lookup"><span data-stu-id="bd0de-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="bd0de-118">Le masquage d’un élément d’interface utilisateur peuvent ne pas complètement empêche l’accès à son action de contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="bd0de-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="bd0de-119">Par exemple, considérez le bouton dans l’extrait de code précédent.</span><span class="sxs-lookup"><span data-stu-id="bd0de-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="bd0de-120">Un utilisateur peut appeler le `Edit` URL de la méthode d’action si il connaît la ressource relative est */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="bd0de-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="bd0de-121">Pour cette raison, la `Edit` méthode d’action doit effectuer son propre contrôle d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="bd0de-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
