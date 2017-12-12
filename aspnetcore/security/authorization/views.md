---
title: "Autorisation basée sur le mode dans ASP.NET MVC de base"
author: rick-anderson
description: "Ce document montre comment injecter et utiliser le service d’autorisation à l’intérieur d’une vue ASP.NET Core Razor."
keywords: "ASP.NET Core, l’autorisation de l’autorisation, IAuthorizationService, Razor"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="b76a6-104">Autorisation basée sur une vue</span><span class="sxs-lookup"><span data-stu-id="b76a6-104">View-based authorization</span></span>

<span data-ttu-id="b76a6-105">Un développeur souhaite souvent afficher, masquer ou modifier une interface utilisateur basée sur l’identité de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="b76a6-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="b76a6-106">Vous pouvez accéder au service de l’autorisation au sein de vues MVC via [injection de dépendance](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b76a6-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="b76a6-107">Pour insérer le service d’autorisation dans une vue Razor, utilisez la `@inject` directive :</span><span class="sxs-lookup"><span data-stu-id="b76a6-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="b76a6-108">Si vous souhaitez que le service d’autorisation dans chaque vue, placez le `@inject` directive dans le *_ViewImports.cshtml* fichier de la *vues* active.</span><span class="sxs-lookup"><span data-stu-id="b76a6-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="b76a6-109">Pour plus d’informations, consultez [injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b76a6-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="b76a6-110">Permet d’appeler le service d’autorisation injecté `AuthorizeAsync` dans la même façon, vous devez vérifier pendant [d’autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="b76a6-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b76a6-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b76a6-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b76a6-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b76a6-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="b76a6-113">Dans certains cas, la ressource sera votre modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="b76a6-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="b76a6-114">Appeler `AuthorizeAsync` dans la même façon, vous devez vérifier pendant [d’autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="b76a6-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b76a6-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b76a6-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b76a6-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b76a6-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="b76a6-117">Dans le code précédent, le modèle est transmis en tant que ressource que l’évaluation de stratégie doit prendre en considération.</span><span class="sxs-lookup"><span data-stu-id="b76a6-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="b76a6-118">Ne vous fiez pas bascule de visibilité des éléments d’interface utilisateur de votre application en tant que le contrôle d’autorisation unique.</span><span class="sxs-lookup"><span data-stu-id="b76a6-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="b76a6-119">Le masquage d’un élément d’interface utilisateur peuvent ne pas complètement empêche l’accès à son action de contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="b76a6-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="b76a6-120">Par exemple, considérez le bouton dans l’extrait de code précédent.</span><span class="sxs-lookup"><span data-stu-id="b76a6-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="b76a6-121">Un utilisateur peut appeler le `Edit` URL de la méthode d’action si il connaît la ressource relative est */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="b76a6-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="b76a6-122">Pour cette raison, la `Edit` méthode d’action doit effectuer son propre contrôle d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="b76a6-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
