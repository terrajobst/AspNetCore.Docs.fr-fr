---
title: Autorisation basée sur les vues dans ASP.NET Core MVC
author: rick-anderson
description: Ce document montre comment injecter et utiliser le service d’autorisation à l’intérieur d’une vue Razor ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663596"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="1ef13-103">Autorisation basée sur les vues dans ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="1ef13-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="1ef13-104">Un développeur souhaite souvent afficher, masquer ou modifier une interface utilisateur basée sur l’identité de l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="1ef13-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="1ef13-105">Vous pouvez accéder au service d’autorisation dans les vues MVC via l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1ef13-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1ef13-106">Pour injecter le service d’autorisation dans une vue Razor, utilisez la directive `@inject` :</span><span class="sxs-lookup"><span data-stu-id="1ef13-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="1ef13-107">Si vous souhaitez utiliser le service d’autorisation dans chaque vue, placez la directive `@inject` dans le fichier *_ViewImports. cshtml* du répertoire *views* .</span><span class="sxs-lookup"><span data-stu-id="1ef13-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="1ef13-108">Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1ef13-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="1ef13-109">Utilisez le service d’autorisation injecté pour appeler `AuthorizeAsync` exactement de la même façon que lors de l' [autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="1ef13-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="1ef13-110">Dans certains cas, la ressource sera votre modèle de Vue.</span><span class="sxs-lookup"><span data-stu-id="1ef13-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="1ef13-111">Appelez `AuthorizeAsync` exactement de la même façon que lors de la vérification de l' [autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="1ef13-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="1ef13-112">Dans le code précédent, le modèle est passé en tant que ressource que l’évaluation de la stratégie doit prendre en compte.</span><span class="sxs-lookup"><span data-stu-id="1ef13-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="1ef13-113">Ne vous fiez pas au basculement de la visibilité des éléments d’interface utilisateur de votre application en tant que seul contrôle d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="1ef13-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="1ef13-114">Le masquage d’un élément d’interface utilisateur peut ne pas empêcher complètement l’accès à son action de contrôleur associée.</span><span class="sxs-lookup"><span data-stu-id="1ef13-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="1ef13-115">Par exemple, considérez le bouton dans l’extrait de code précédent.</span><span class="sxs-lookup"><span data-stu-id="1ef13-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="1ef13-116">Un utilisateur peut appeler la méthode d’action `Edit` s’il connaît l’URL de ressource relative */document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="1ef13-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="1ef13-117">C’est la raison pour laquelle la méthode d’action `Edit` doit effectuer sa propre vérification d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="1ef13-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
