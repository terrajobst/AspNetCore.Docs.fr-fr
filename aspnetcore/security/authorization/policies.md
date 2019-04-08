---
title: Autorisation basée sur des stratégies dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer et utiliser des gestionnaires de stratégie d’autorisation pour l’application des exigences d’autorisation dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068168"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="95c6c-103">Autorisation basée sur des stratégies dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95c6c-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="95c6c-104">Dans les coulisses, [autorisation basée sur le rôle](xref:security/authorization/roles) et [autorisation basée sur les revendications](xref:security/authorization/claims) utilisent une exigence, un gestionnaire de condition et une stratégie préconfigurée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="95c6c-105">Ces blocs de construction prend en charge l’expression d’évaluations d’autorisation dans le code.</span><span class="sxs-lookup"><span data-stu-id="95c6c-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="95c6c-106">Le résultat est une structure d’autorisation plus riches, réutilisables et faciles à tester.</span><span class="sxs-lookup"><span data-stu-id="95c6c-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="95c6c-107">Une stratégie d’autorisation se compose d’une ou plusieurs exigences.</span><span class="sxs-lookup"><span data-stu-id="95c6c-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="95c6c-108">Il est inscrit dans le cadre de la configuration du service d’autorisation, dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="95c6c-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="95c6c-109">Dans l’exemple précédent, une stratégie de « AtLeast21 » est créée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="95c6c-110">Il a une seule exigence&mdash;celle d’un minimum d’ancienneté, qui est fourni en tant que paramètre à cette exigence.</span><span class="sxs-lookup"><span data-stu-id="95c6c-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="95c6c-111">Appliquer des stratégies à des contrôleurs MVC</span><span class="sxs-lookup"><span data-stu-id="95c6c-111">Applying policies to MVC controllers</span></span>

<span data-ttu-id="95c6c-112">Si vous utilisez des Pages Razor, consultez [appliquer des stratégies aux Pages Razor](#applying-policies-to-razor-pages) dans ce document.</span><span class="sxs-lookup"><span data-stu-id="95c6c-112">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="95c6c-113">Les stratégies sont appliquées aux contrôleurs à l’aide de la `[Authorize]` attribut avec le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-113">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="95c6c-114">Exemple :</span><span class="sxs-lookup"><span data-stu-id="95c6c-114">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="95c6c-115">Appliquer des stratégies aux Pages Razor</span><span class="sxs-lookup"><span data-stu-id="95c6c-115">Applying policies to Razor Pages</span></span>

<span data-ttu-id="95c6c-116">Les stratégies sont appliquées aux Pages Razor à l’aide de la `[Authorize]` attribut avec le nom de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-116">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="95c6c-117">Exemple :</span><span class="sxs-lookup"><span data-stu-id="95c6c-117">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="95c6c-118">Stratégies peuvent également être appliquées aux Pages Razor en utilisant un [convention d’autorisation](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="95c6c-118">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="95c6c-119">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="95c6c-119">Requirements</span></span>

<span data-ttu-id="95c6c-120">Une condition d’autorisation est une collection de paramètres de données une stratégie peut utiliser pour évaluer le principal utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="95c6c-120">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="95c6c-121">Dans notre stratégie de « AtLeast21 », l’exigence est un paramètre unique&mdash;l’ancienneté minimale.</span><span class="sxs-lookup"><span data-stu-id="95c6c-121">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="95c6c-122">Implémente une exigence [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), qui est une interface de marqueur vide.</span><span class="sxs-lookup"><span data-stu-id="95c6c-122">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="95c6c-123">Une exigence paramétrable ancienneté minimale peut être implémentée comme suit :</span><span class="sxs-lookup"><span data-stu-id="95c6c-123">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="95c6c-124">Si une stratégie d’autorisation contient plusieurs exigences d’autorisation, toutes les conditions requises doivent passer pour l’évaluation de stratégie réussisse.</span><span class="sxs-lookup"><span data-stu-id="95c6c-124">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="95c6c-125">En d’autres termes, plusieurs exigences d’autorisation ajoutés à une stratégie d’autorisation unique sont traités sur un **AND** base.</span><span class="sxs-lookup"><span data-stu-id="95c6c-125">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="95c6c-126">Une exigence n’a pas besoin d’avoir des propriétés ou des données.</span><span class="sxs-lookup"><span data-stu-id="95c6c-126">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="95c6c-127">Gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="95c6c-127">Authorization handlers</span></span>

<span data-ttu-id="95c6c-128">Un gestionnaire d’autorisation est responsable de l’évaluation des propriétés d’une exigence.</span><span class="sxs-lookup"><span data-stu-id="95c6c-128">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="95c6c-129">Le Gestionnaire d’autorisation évalue la configuration requise par rapport à un fourni [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) pour déterminer si l’accès est autorisé.</span><span class="sxs-lookup"><span data-stu-id="95c6c-129">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="95c6c-130">Une condition peut avoir [plusieurs gestionnaires](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="95c6c-130">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="95c6c-131">Un gestionnaire peut hériter [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), où `TRequirement` est la nécessité d’être gérée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-131">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="95c6c-132">Vous pouvez également, peut implémenter un gestionnaire [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pour gérer plusieurs types de spécifications.</span><span class="sxs-lookup"><span data-stu-id="95c6c-132">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="95c6c-133">Utiliser un gestionnaire pour une exigence</span><span class="sxs-lookup"><span data-stu-id="95c6c-133">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="95c6c-134">Voici un exemple d’une relation un à un dans lequel un gestionnaire de l’ancienneté minimale utilise une seule exigence :</span><span class="sxs-lookup"><span data-stu-id="95c6c-134">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="95c6c-135">Le code précédent détermine si l’utilisateur principal a une date de naissance de revendication qui a été émis par un émetteur connu et approuvé.</span><span class="sxs-lookup"><span data-stu-id="95c6c-135">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="95c6c-136">L’autorisation ne peut pas se produire lors de la revendication est manquante, auquel cas une tâche achevée est retournée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-136">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="95c6c-137">Lorsqu’une revendication est présente, l’âge de l’utilisateur est calculée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-137">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="95c6c-138">Si l’utilisateur remplit l’ancienneté minimale définie par la configuration requise, l’autorisation est considérée comme réussie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-138">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="95c6c-139">Lors de l’autorisation est accordée, `context.Succeed` est appelé avec l’exigence satisfait par son seul paramètre.</span><span class="sxs-lookup"><span data-stu-id="95c6c-139">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="95c6c-140">Utiliser un gestionnaire pour plusieurs conditions</span><span class="sxs-lookup"><span data-stu-id="95c6c-140">Use a handler for multiple requirements</span></span>

<span data-ttu-id="95c6c-141">Voici un exemple d’une relation un-à-plusieurs dans lequel un gestionnaire d’autorisation permettre gérer trois différents types de conditions :</span><span class="sxs-lookup"><span data-stu-id="95c6c-141">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="95c6c-142">Le code précédent traverse [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;une propriété contenant des exigences ne pas marquée comme ayant réussie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-142">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="95c6c-143">Pour un `ReadPermission` exigence, l’utilisateur doit être un propriétaire ou un commanditaire pour accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-143">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="95c6c-144">Dans le cas d’un `EditPermission` ou `DeletePermission` exigence, il doit être un propriétaire à accéder à la ressource demandée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-144">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="95c6c-145">Inscription du Gestionnaire</span><span class="sxs-lookup"><span data-stu-id="95c6c-145">Handler registration</span></span>

<span data-ttu-id="95c6c-146">Gestionnaires sont enregistrés dans la collection de services lors de la configuration.</span><span class="sxs-lookup"><span data-stu-id="95c6c-146">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="95c6c-147">Exemple :</span><span class="sxs-lookup"><span data-stu-id="95c6c-147">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="95c6c-148">Inscrit le code précédent `MinimumAgeHandler` comme un singleton en appelant `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="95c6c-148">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="95c6c-149">Gestionnaires peuvent être enregistrés à l’aide d’intégrés [des durées de vie du service](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="95c6c-149">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="95c6c-150">Ce qui doit retourner un gestionnaire ?</span><span class="sxs-lookup"><span data-stu-id="95c6c-150">What should a handler return?</span></span>

<span data-ttu-id="95c6c-151">Notez que le `Handle` méthode dans le [handler, exemple](#security-authorization-handler-example) ne retourne aucune valeur.</span><span class="sxs-lookup"><span data-stu-id="95c6c-151">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="95c6c-152">Comment est un état de réussite ou échec indiqué ?</span><span class="sxs-lookup"><span data-stu-id="95c6c-152">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="95c6c-153">Un gestionnaire indique la réussite en appelant `context.Succeed(IAuthorizationRequirement requirement)`, en passant l’exigence qui a été validé avec succès.</span><span class="sxs-lookup"><span data-stu-id="95c6c-153">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="95c6c-154">Un gestionnaire n’a pas besoin de gérer les défaillances en règle générale, comme les autres gestionnaires pour les mêmes exigences peuvent réussir.</span><span class="sxs-lookup"><span data-stu-id="95c6c-154">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="95c6c-155">Pour garantir la défaillance, même si d’autres gestionnaires d’exigences réussissent, appelez `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="95c6c-155">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="95c6c-156">Si un gestionnaire appelle `context.Succeed` ou `context.Fail`, tous les autres gestionnaires sont toujours appelés.</span><span class="sxs-lookup"><span data-stu-id="95c6c-156">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="95c6c-157">Ainsi, afin de produire des effets secondaires, tels que la journalisation, qui a lieu même si un autre gestionnaire a correctement validé ou a échoué une exigence.</span><span class="sxs-lookup"><span data-stu-id="95c6c-157">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="95c6c-158">Lorsque la valeur `false`, le [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propriété (disponible dans ASP.NET Core 1.1 et versions ultérieure) court-circuite l’exécution des gestionnaires lorsque `context.Fail` est appelée.</span><span class="sxs-lookup"><span data-stu-id="95c6c-158">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> `InvokeHandlersAfterFailure` <span data-ttu-id="95c6c-159">valeur par défaut est `true`, auquel cas tous les gestionnaires sont appelés.</span><span class="sxs-lookup"><span data-stu-id="95c6c-159">defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="95c6c-160">Les gestionnaires d’autorisation sont appelés même si l’authentification échoue.</span><span class="sxs-lookup"><span data-stu-id="95c6c-160">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="95c6c-161">Pourquoi devrais-je plusieurs gestionnaires pour une spécification ?</span><span class="sxs-lookup"><span data-stu-id="95c6c-161">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="95c6c-162">Dans les cas où vous souhaitez d’évaluation doivent se trouver sur un **ou** base, implémenter plusieurs gestionnaires pour une spécification unique.</span><span class="sxs-lookup"><span data-stu-id="95c6c-162">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="95c6c-163">Par exemple, Microsoft a les portes n’ouvrir avec des cartes de clé.</span><span class="sxs-lookup"><span data-stu-id="95c6c-163">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="95c6c-164">Si vous laissez votre carte de clé à la maison, la réceptionniste imprime un autocollant temporaire et ouvre la porte pour vous.</span><span class="sxs-lookup"><span data-stu-id="95c6c-164">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="95c6c-165">Dans ce scénario, il vous aurait une seule exigence, *BuildingEntry*, mais plusieurs gestionnaires, chacun d'entre eux examinant une seule condition.</span><span class="sxs-lookup"><span data-stu-id="95c6c-165">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

*<span data-ttu-id="95c6c-166">BuildingEntryRequirement.cs</span><span class="sxs-lookup"><span data-stu-id="95c6c-166">BuildingEntryRequirement.cs</span></span>*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*<span data-ttu-id="95c6c-167">BadgeEntryHandler.cs</span><span class="sxs-lookup"><span data-stu-id="95c6c-167">BadgeEntryHandler.cs</span></span>*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*<span data-ttu-id="95c6c-168">TemporaryStickerHandler.cs</span><span class="sxs-lookup"><span data-stu-id="95c6c-168">TemporaryStickerHandler.cs</span></span>*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="95c6c-169">Assurez-vous que les deux gestionnaires sont [inscrit](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="95c6c-169">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="95c6c-170">Si un gestionnaire réussit lorsqu’une stratégie évalue la `BuildingEntryRequirement`, réussit l’évaluation de stratégie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-170">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="95c6c-171">À l’aide d’une variable func pour répondre à une stratégie</span><span class="sxs-lookup"><span data-stu-id="95c6c-171">Using a func to fulfill a policy</span></span>

<span data-ttu-id="95c6c-172">Il peut arriver dans quel remplissant une stratégie est simple à exprimer dans le code.</span><span class="sxs-lookup"><span data-stu-id="95c6c-172">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="95c6c-173">Il est possible de fournir un `Func<AuthorizationHandlerContext, bool>` lorsque vous configurez votre stratégie avec le `RequireAssertion` Générateur de stratégie.</span><span class="sxs-lookup"><span data-stu-id="95c6c-173">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="95c6c-174">Par exemple, le précédent `BadgeEntryHandler` peut être réécrit comme suit :</span><span class="sxs-lookup"><span data-stu-id="95c6c-174">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="95c6c-175">Accès au contexte de demande MVC dans les gestionnaires</span><span class="sxs-lookup"><span data-stu-id="95c6c-175">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="95c6c-176">Le `HandleRequirementAsync` méthode que vous implémentez dans un gestionnaire d’autorisation a deux paramètres : un `AuthorizationHandlerContext` et `TRequirement` que vous gérez.</span><span class="sxs-lookup"><span data-stu-id="95c6c-176">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="95c6c-177">Infrastructures telles que MVC ou Jabbr sont libres d’ajouter n’importe quel objet à la `Resource` propriété sur le `AuthorizationHandlerContext` pour passer des informations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="95c6c-177">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="95c6c-178">Par exemple, MVC passe une instance de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) dans le `Resource` propriété.</span><span class="sxs-lookup"><span data-stu-id="95c6c-178">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="95c6c-179">Cette propriété fournit l’accès aux `HttpContext`, `RouteData`et tout autre fournie par MVC et les Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="95c6c-179">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="95c6c-180">L’utilisation de la `Resource` propriété est framework spécifique.</span><span class="sxs-lookup"><span data-stu-id="95c6c-180">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="95c6c-181">À l’aide des informations contenues dans le `Resource` propriété limite vos stratégies d’autorisation aux infrastructures particuliers.</span><span class="sxs-lookup"><span data-stu-id="95c6c-181">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="95c6c-182">Vous devez effectuer un cast du `Resource` à l’aide de la propriété le `is` mot clé, puis confirmez la conversion a réussi pour vérifier votre code ne se bloque avec une `InvalidCastException` lorsque vous travaillez sur d’autres infrastructures :</span><span class="sxs-lookup"><span data-stu-id="95c6c-182">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
