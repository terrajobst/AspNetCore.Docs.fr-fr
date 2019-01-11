---
title: Autorisation basée sur des stratégies dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer et utiliser des gestionnaires de stratégie d’autorisation pour l’application des exigences d’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 4e8a9ac6c0594f9bab67214aaa8cab9199cca29d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207393"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="97751-103">Autorisation basée sur des stratégies dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97751-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="97751-104">Dans les coulisses, [autorisation basée sur le rôle](xref:security/authorization/roles) et [autorisation basée sur les revendications](xref:security/authorization/claims) utilisent une exigence, un gestionnaire de condition et une stratégie préconfigurée.</span><span class="sxs-lookup"><span data-stu-id="97751-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="97751-105">Ces blocs de construction prend en charge l’expression d’évaluations d’autorisation dans le code.</span><span class="sxs-lookup"><span data-stu-id="97751-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="97751-106">Le résultat est une structure d’autorisation plus riches, réutilisables et faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="97751-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="97751-107">Une stratégie d’autorisation se compose d’une ou plusieurs exigences.</span><span class="sxs-lookup"><span data-stu-id="97751-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="97751-108">Il est inscrit dans le cadre de la configuration du service d’autorisation, dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="97751-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="97751-109">Dans l’exemple précédent, une stratégie de « AtLeast21 » est créée.</span><span class="sxs-lookup"><span data-stu-id="97751-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="97751-110">Il a une seule exigence&mdash;celle d’un minimum d’ancienneté, qui est fourni en tant que paramètre à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="97751-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="97751-111">Les stratégies sont appliquées à l’aide de la `[Authorize]` attribut avec le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="97751-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="97751-112">Exemple :</span><span class="sxs-lookup"><span data-stu-id="97751-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="97751-113">Spécifications</span><span class="sxs-lookup"><span data-stu-id="97751-113">Requirements</span></span>

<span data-ttu-id="97751-114">Une condition d’autorisation est une collection de paramètres de données une stratégie peut utiliser pour évaluer le principal utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="97751-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="97751-115">Dans notre stratégie de « AtLeast21 », l’exigence est un paramètre unique&mdash;l’ancienneté minimale.</span><span class="sxs-lookup"><span data-stu-id="97751-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="97751-116">Implémente une exigence [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), qui est une interface de marqueur vide.</span><span class="sxs-lookup"><span data-stu-id="97751-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="97751-117">Une exigence paramétrable ancienneté minimale peut être implémentée comme suit :</span><span class="sxs-lookup"><span data-stu-id="97751-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="97751-118">Une exigence n’a pas besoin d’avoir des propriétés ou des données.</span><span class="sxs-lookup"><span data-stu-id="97751-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="97751-119">Gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="97751-119">Authorization handlers</span></span>

<span data-ttu-id="97751-120">Un gestionnaire d’autorisation est responsable de l’évaluation des propriétés d’une exigence.</span><span class="sxs-lookup"><span data-stu-id="97751-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="97751-121">Le Gestionnaire d’autorisation évalue la configuration requise par rapport à un fourni [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) pour déterminer si l’accès est autorisé.</span><span class="sxs-lookup"><span data-stu-id="97751-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="97751-122">Une condition peut avoir [plusieurs gestionnaires](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="97751-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="97751-123">Un gestionnaire peut hériter [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), où `TRequirement` est la nécessité d’être gérée.</span><span class="sxs-lookup"><span data-stu-id="97751-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="97751-124">Vous pouvez également, peut implémenter un gestionnaire [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pour gérer plusieurs types de spécifications.</span><span class="sxs-lookup"><span data-stu-id="97751-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="97751-125">Utiliser un gestionnaire pour une exigence</span><span class="sxs-lookup"><span data-stu-id="97751-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="97751-126">Voici un exemple d’une relation un à un dans lequel un gestionnaire de l’ancienneté minimale utilise une seule exigence :</span><span class="sxs-lookup"><span data-stu-id="97751-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="97751-127">Le code précédent détermine si l’utilisateur principal a une date de naissance de revendication qui a été émis par un émetteur connu et approuvé.</span><span class="sxs-lookup"><span data-stu-id="97751-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="97751-128">L’autorisation ne peut pas se produire lors de la revendication est manquante, auquel cas une tâche achevée est retournée.</span><span class="sxs-lookup"><span data-stu-id="97751-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="97751-129">Lorsqu’une revendication est présente, l’âge de l’utilisateur est calculée.</span><span class="sxs-lookup"><span data-stu-id="97751-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="97751-130">Si l’utilisateur remplit l’ancienneté minimale définie par la configuration requise, l’autorisation est considérée comme réussie.</span><span class="sxs-lookup"><span data-stu-id="97751-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="97751-131">Lors de l’autorisation est accordée, `context.Succeed` est appelé avec l’exigence satisfait par son seul paramètre.</span><span class="sxs-lookup"><span data-stu-id="97751-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="97751-132">Utiliser un gestionnaire pour plusieurs conditions</span><span class="sxs-lookup"><span data-stu-id="97751-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="97751-133">Voici un exemple d’une relation un-à-plusieurs dans lequel un gestionnaire d’autorisation permettre gérer trois différents types de conditions :</span><span class="sxs-lookup"><span data-stu-id="97751-133">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="97751-134">Le code précédent traverse [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;une propriété contenant des exigences ne pas marquée comme ayant réussie.</span><span class="sxs-lookup"><span data-stu-id="97751-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="97751-135">Pour un `ReadPermission` exigence, l’utilisateur doit être un propriétaire ou un commanditaire pour accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="97751-135">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="97751-136">Dans le cas d’un `EditPermission` ou `DeletePermission` exigence, il doit être un propriétaire à accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="97751-136">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="97751-137">Inscription du Gestionnaire</span><span class="sxs-lookup"><span data-stu-id="97751-137">Handler registration</span></span>

<span data-ttu-id="97751-138">Gestionnaires sont enregistrés dans la collection de services lors de la configuration.</span><span class="sxs-lookup"><span data-stu-id="97751-138">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="97751-139">Exemple :</span><span class="sxs-lookup"><span data-stu-id="97751-139">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="97751-140">Chaque gestionnaire est ajouté à la collection de services en appelant `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="97751-140">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="97751-141">Ce qui doit retourner un gestionnaire ?</span><span class="sxs-lookup"><span data-stu-id="97751-141">What should a handler return?</span></span>

<span data-ttu-id="97751-142">Notez que le `Handle` méthode dans le [handler, exemple](#security-authorization-handler-example) ne retourne aucune valeur.</span><span class="sxs-lookup"><span data-stu-id="97751-142">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="97751-143">Comment est un état de réussite ou échec indiqué ?</span><span class="sxs-lookup"><span data-stu-id="97751-143">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="97751-144">Un gestionnaire indique la réussite en appelant `context.Succeed(IAuthorizationRequirement requirement)`, en passant l’exigence qui a été validé avec succès.</span><span class="sxs-lookup"><span data-stu-id="97751-144">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="97751-145">Un gestionnaire n’a pas besoin de gérer les défaillances en règle générale, comme les autres gestionnaires pour les mêmes exigences peuvent réussir.</span><span class="sxs-lookup"><span data-stu-id="97751-145">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="97751-146">Pour garantir la défaillance, même si d’autres gestionnaires d’exigences réussissent, appelez `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="97751-146">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="97751-147">Lorsque la valeur `false`, le [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propriété (disponible dans ASP.NET Core 1.1 et versions ultérieure) court-circuite l’exécution des gestionnaires lorsque `context.Fail` est appelée.</span><span class="sxs-lookup"><span data-stu-id="97751-147">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="97751-148">`InvokeHandlersAfterFailure` valeur par défaut est `true`, auquel cas tous les gestionnaires sont appelés.</span><span class="sxs-lookup"><span data-stu-id="97751-148">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="97751-149">Ainsi, les conditions requises pour produire des effets secondaires, tels que la journalisation, qui ont toujours lieu même si `context.Fail` a été appelée dans un autre gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="97751-149">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="97751-150">Pourquoi devrais-je plusieurs gestionnaires pour une spécification ?</span><span class="sxs-lookup"><span data-stu-id="97751-150">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="97751-151">Dans les cas où vous souhaitez d’évaluation doivent se trouver sur un **ou** base, implémenter plusieurs gestionnaires pour une spécification unique.</span><span class="sxs-lookup"><span data-stu-id="97751-151">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="97751-152">Par exemple, Microsoft a les portes n’ouvrir avec des cartes de clé.</span><span class="sxs-lookup"><span data-stu-id="97751-152">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="97751-153">Si vous laissez votre carte de clé à la maison, la réceptionniste imprime un autocollant temporaire et ouvre la porte pour vous.</span><span class="sxs-lookup"><span data-stu-id="97751-153">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="97751-154">Dans ce scénario, il vous aurait une seule exigence, *BuildingEntry*, mais plusieurs gestionnaires, chacun d'entre eux examinant une seule condition.</span><span class="sxs-lookup"><span data-stu-id="97751-154">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="97751-155">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="97751-155">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="97751-156">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="97751-156">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="97751-157">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="97751-157">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="97751-158">Assurez-vous que les deux gestionnaires sont [inscrit](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="97751-158">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="97751-159">Si un gestionnaire réussit lorsqu’une stratégie évalue la `BuildingEntryRequirement`, réussit l’évaluation de stratégie.</span><span class="sxs-lookup"><span data-stu-id="97751-159">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="97751-160">À l’aide d’une variable func pour répondre à une stratégie</span><span class="sxs-lookup"><span data-stu-id="97751-160">Using a func to fulfill a policy</span></span>

<span data-ttu-id="97751-161">Il peut arriver dans quel remplissant une stratégie est simple à exprimer dans le code.</span><span class="sxs-lookup"><span data-stu-id="97751-161">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="97751-162">Il est possible de fournir un `Func<AuthorizationHandlerContext, bool>` lorsque vous configurez votre stratégie avec le `RequireAssertion` Générateur de stratégie.</span><span class="sxs-lookup"><span data-stu-id="97751-162">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="97751-163">Par exemple, le précédent `BadgeEntryHandler` peut être réécrit comme suit :</span><span class="sxs-lookup"><span data-stu-id="97751-163">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="97751-164">Accès au contexte de demande MVC dans les gestionnaires</span><span class="sxs-lookup"><span data-stu-id="97751-164">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="97751-165">Le `HandleRequirementAsync` méthode que vous implémentez dans un gestionnaire d’autorisation a deux paramètres : un `AuthorizationHandlerContext` et `TRequirement` que vous gérez.</span><span class="sxs-lookup"><span data-stu-id="97751-165">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="97751-166">Infrastructures telles que MVC ou Jabbr sont libres d’ajouter n’importe quel objet à la `Resource` propriété sur le `AuthorizationHandlerContext` pour passer des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="97751-166">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="97751-167">Par exemple, MVC passe une instance de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) dans le `Resource` propriété.</span><span class="sxs-lookup"><span data-stu-id="97751-167">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="97751-168">Cette propriété fournit l’accès aux `HttpContext`, `RouteData`et tout autre fournie par MVC et les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="97751-168">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="97751-169">L’utilisation de la `Resource` propriété est framework spécifique.</span><span class="sxs-lookup"><span data-stu-id="97751-169">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="97751-170">À l’aide des informations contenues dans le `Resource` propriété limite vos stratégies d’autorisation aux infrastructures particuliers.</span><span class="sxs-lookup"><span data-stu-id="97751-170">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="97751-171">Vous devez effectuer un cast du `Resource` à l’aide de la propriété le `as` mot clé, puis confirmez la conversion a été pour vérifier votre code ne se bloque avec une `InvalidCastException` lorsque vous travaillez sur d’autres infrastructures :</span><span class="sxs-lookup"><span data-stu-id="97751-171">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
