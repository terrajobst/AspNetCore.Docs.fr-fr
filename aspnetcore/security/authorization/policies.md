---
title: Autorisation basée sur des stratégies dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer et utiliser des gestionnaires de stratégie d’autorisation pour l’application des exigences d’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: be4812487c92a16c44e3983b234bc9e31be65190
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410387"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="75cc2-103">Autorisation basée sur des stratégies dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75cc2-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="75cc2-104">Dans les coulisses, [autorisation basée sur le rôle](xref:security/authorization/roles) et [autorisation basée sur les revendications](xref:security/authorization/claims) utilisent une exigence, un gestionnaire de condition et une stratégie préconfigurée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="75cc2-105">Ces blocs de construction prend en charge l’expression d’évaluations d’autorisation dans le code.</span><span class="sxs-lookup"><span data-stu-id="75cc2-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="75cc2-106">Le résultat est une structure d’autorisation plus riches, réutilisables et faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="75cc2-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="75cc2-107">Une stratégie d’autorisation se compose d’une ou plusieurs exigences.</span><span class="sxs-lookup"><span data-stu-id="75cc2-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="75cc2-108">Il est inscrit dans le cadre de la configuration du service d’autorisation, dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="75cc2-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="75cc2-109">Dans l’exemple précédent, une stratégie de « AtLeast21 » est créée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="75cc2-110">Il a une seule exigence&mdash;celle d’un minimum d’ancienneté, qui est fourni en tant que paramètre à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="75cc2-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="75cc2-111">Les stratégies sont appliquées à l’aide de la `[Authorize]` attribut avec le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="75cc2-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="75cc2-112">Exemple :</span><span class="sxs-lookup"><span data-stu-id="75cc2-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="75cc2-113">Spécifications</span><span class="sxs-lookup"><span data-stu-id="75cc2-113">Requirements</span></span>

<span data-ttu-id="75cc2-114">Une condition d’autorisation est une collection de paramètres de données une stratégie peut utiliser pour évaluer le principal utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="75cc2-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="75cc2-115">Dans notre stratégie de « AtLeast21 », l’exigence est un paramètre unique&mdash;l’ancienneté minimale.</span><span class="sxs-lookup"><span data-stu-id="75cc2-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="75cc2-116">Implémente une exigence [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), qui est une interface de marqueur vide.</span><span class="sxs-lookup"><span data-stu-id="75cc2-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="75cc2-117">Une exigence paramétrable ancienneté minimale peut être implémentée comme suit :</span><span class="sxs-lookup"><span data-stu-id="75cc2-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="75cc2-118">Si une stratégie d’autorisation contient plusieurs exigences d’autorisation, toutes les conditions requises doivent passer pour l’évaluation de stratégie réussisse.</span><span class="sxs-lookup"><span data-stu-id="75cc2-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="75cc2-119">En d’autres termes, plusieurs exigences d’autorisation ajoutés à une stratégie d’autorisation unique sont traités sur un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="75cc2-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="75cc2-120">Une exigence n’a pas besoin d’avoir des propriétés ou des données.</span><span class="sxs-lookup"><span data-stu-id="75cc2-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="75cc2-121">Gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="75cc2-121">Authorization handlers</span></span>

<span data-ttu-id="75cc2-122">Un gestionnaire d’autorisation est responsable de l’évaluation des propriétés d’une exigence.</span><span class="sxs-lookup"><span data-stu-id="75cc2-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="75cc2-123">Le Gestionnaire d’autorisation évalue la configuration requise par rapport à un fourni [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) pour déterminer si l’accès est autorisé.</span><span class="sxs-lookup"><span data-stu-id="75cc2-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="75cc2-124">Une condition peut avoir [plusieurs gestionnaires](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="75cc2-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="75cc2-125">Un gestionnaire peut hériter [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), où `TRequirement` est la nécessité d’être gérée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="75cc2-126">Vous pouvez également, peut implémenter un gestionnaire [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pour gérer plusieurs types de spécifications.</span><span class="sxs-lookup"><span data-stu-id="75cc2-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="75cc2-127">Utiliser un gestionnaire pour une exigence</span><span class="sxs-lookup"><span data-stu-id="75cc2-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="75cc2-128">Voici un exemple d’une relation un à un dans lequel un gestionnaire de l’ancienneté minimale utilise une seule exigence :</span><span class="sxs-lookup"><span data-stu-id="75cc2-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="75cc2-129">Le code précédent détermine si l’utilisateur principal a une date de naissance de revendication qui a été émis par un émetteur connu et approuvé.</span><span class="sxs-lookup"><span data-stu-id="75cc2-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="75cc2-130">L’autorisation ne peut pas se produire lors de la revendication est manquante, auquel cas une tâche achevée est retournée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="75cc2-131">Lorsqu’une revendication est présente, l’âge de l’utilisateur est calculée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="75cc2-132">Si l’utilisateur remplit l’ancienneté minimale définie par la configuration requise, l’autorisation est considérée comme réussie.</span><span class="sxs-lookup"><span data-stu-id="75cc2-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="75cc2-133">Lors de l’autorisation est accordée, `context.Succeed` est appelé avec l’exigence satisfait par son seul paramètre.</span><span class="sxs-lookup"><span data-stu-id="75cc2-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="75cc2-134">Utiliser un gestionnaire pour plusieurs conditions</span><span class="sxs-lookup"><span data-stu-id="75cc2-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="75cc2-135">Voici un exemple d’une relation un-à-plusieurs dans lequel un gestionnaire d’autorisation permettre gérer trois différents types de conditions :</span><span class="sxs-lookup"><span data-stu-id="75cc2-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="75cc2-136">Le code précédent traverse [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;une propriété contenant des exigences ne pas marquée comme ayant réussie.</span><span class="sxs-lookup"><span data-stu-id="75cc2-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="75cc2-137">Pour un `ReadPermission` exigence, l’utilisateur doit être un propriétaire ou un commanditaire pour accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="75cc2-138">Dans le cas d’un `EditPermission` ou `DeletePermission` exigence, il doit être un propriétaire à accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="75cc2-139">Inscription du Gestionnaire</span><span class="sxs-lookup"><span data-stu-id="75cc2-139">Handler registration</span></span>

<span data-ttu-id="75cc2-140">Gestionnaires sont enregistrés dans la collection de services lors de la configuration.</span><span class="sxs-lookup"><span data-stu-id="75cc2-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="75cc2-141">Exemple :</span><span class="sxs-lookup"><span data-stu-id="75cc2-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="75cc2-142">Chaque gestionnaire est ajouté à la collection de services en appelant `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="75cc2-142">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="75cc2-143">Ce qui doit retourner un gestionnaire ?</span><span class="sxs-lookup"><span data-stu-id="75cc2-143">What should a handler return?</span></span>

<span data-ttu-id="75cc2-144">Notez que le `Handle` méthode dans le [handler, exemple](#security-authorization-handler-example) ne retourne aucune valeur.</span><span class="sxs-lookup"><span data-stu-id="75cc2-144">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="75cc2-145">Comment est un état de réussite ou échec indiqué ?</span><span class="sxs-lookup"><span data-stu-id="75cc2-145">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="75cc2-146">Un gestionnaire indique la réussite en appelant `context.Succeed(IAuthorizationRequirement requirement)`, en passant l’exigence qui a été validé avec succès.</span><span class="sxs-lookup"><span data-stu-id="75cc2-146">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="75cc2-147">Un gestionnaire n’a pas besoin de gérer les défaillances en règle générale, comme les autres gestionnaires pour les mêmes exigences peuvent réussir.</span><span class="sxs-lookup"><span data-stu-id="75cc2-147">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="75cc2-148">Pour garantir la défaillance, même si d’autres gestionnaires d’exigences réussissent, appelez `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="75cc2-148">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="75cc2-149">Lorsque la valeur `false`, le [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propriété (disponible dans ASP.NET Core 1.1 et versions ultérieure) court-circuite l’exécution des gestionnaires lorsque `context.Fail` est appelée.</span><span class="sxs-lookup"><span data-stu-id="75cc2-149">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="75cc2-150">`InvokeHandlersAfterFailure` valeur par défaut est `true`, auquel cas tous les gestionnaires sont appelés.</span><span class="sxs-lookup"><span data-stu-id="75cc2-150">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="75cc2-151">Ainsi, les conditions requises pour produire des effets secondaires, tels que la journalisation, qui ont toujours lieu même si `context.Fail` a été appelée dans un autre gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="75cc2-151">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="75cc2-152">Pourquoi devrais-je plusieurs gestionnaires pour une spécification ?</span><span class="sxs-lookup"><span data-stu-id="75cc2-152">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="75cc2-153">Dans les cas où vous souhaitez d’évaluation doivent se trouver sur un **ou** base, implémenter plusieurs gestionnaires pour une spécification unique.</span><span class="sxs-lookup"><span data-stu-id="75cc2-153">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="75cc2-154">Par exemple, Microsoft a les portes n’ouvrir avec des cartes de clé.</span><span class="sxs-lookup"><span data-stu-id="75cc2-154">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="75cc2-155">Si vous laissez votre carte de clé à la maison, la réceptionniste imprime un autocollant temporaire et ouvre la porte pour vous.</span><span class="sxs-lookup"><span data-stu-id="75cc2-155">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="75cc2-156">Dans ce scénario, il vous aurait une seule exigence, *BuildingEntry*, mais plusieurs gestionnaires, chacun d'entre eux examinant une seule condition.</span><span class="sxs-lookup"><span data-stu-id="75cc2-156">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="75cc2-157">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="75cc2-157">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="75cc2-158">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="75cc2-158">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="75cc2-159">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="75cc2-159">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="75cc2-160">Assurez-vous que les deux gestionnaires sont [inscrit](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="75cc2-160">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="75cc2-161">Si un gestionnaire réussit lorsqu’une stratégie évalue la `BuildingEntryRequirement`, réussit l’évaluation de stratégie.</span><span class="sxs-lookup"><span data-stu-id="75cc2-161">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="75cc2-162">À l’aide d’une variable func pour répondre à une stratégie</span><span class="sxs-lookup"><span data-stu-id="75cc2-162">Using a func to fulfill a policy</span></span>

<span data-ttu-id="75cc2-163">Il peut arriver dans quel remplissant une stratégie est simple à exprimer dans le code.</span><span class="sxs-lookup"><span data-stu-id="75cc2-163">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="75cc2-164">Il est possible de fournir un `Func<AuthorizationHandlerContext, bool>` lorsque vous configurez votre stratégie avec le `RequireAssertion` Générateur de stratégie.</span><span class="sxs-lookup"><span data-stu-id="75cc2-164">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="75cc2-165">Par exemple, le précédent `BadgeEntryHandler` peut être réécrit comme suit :</span><span class="sxs-lookup"><span data-stu-id="75cc2-165">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="75cc2-166">Accès au contexte de demande MVC dans les gestionnaires</span><span class="sxs-lookup"><span data-stu-id="75cc2-166">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="75cc2-167">Le `HandleRequirementAsync` méthode que vous implémentez dans un gestionnaire d’autorisation a deux paramètres : un `AuthorizationHandlerContext` et `TRequirement` que vous gérez.</span><span class="sxs-lookup"><span data-stu-id="75cc2-167">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="75cc2-168">Infrastructures telles que MVC ou Jabbr sont libres d’ajouter n’importe quel objet à la `Resource` propriété sur le `AuthorizationHandlerContext` pour passer des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="75cc2-168">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="75cc2-169">Par exemple, MVC passe une instance de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) dans le `Resource` propriété.</span><span class="sxs-lookup"><span data-stu-id="75cc2-169">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="75cc2-170">Cette propriété fournit l’accès aux `HttpContext`, `RouteData`et tout autre fournie par MVC et les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="75cc2-170">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="75cc2-171">L’utilisation de la `Resource` propriété est framework spécifique.</span><span class="sxs-lookup"><span data-stu-id="75cc2-171">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="75cc2-172">À l’aide des informations contenues dans le `Resource` propriété limite vos stratégies d’autorisation aux infrastructures particuliers.</span><span class="sxs-lookup"><span data-stu-id="75cc2-172">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="75cc2-173">Vous devez effectuer un cast du `Resource` à l’aide de la propriété le `is` mot clé, puis confirmez la conversion a réussi pour vérifier votre code ne se bloque avec une `InvalidCastException` lorsque vous travaillez sur d’autres infrastructures :</span><span class="sxs-lookup"><span data-stu-id="75cc2-173">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
