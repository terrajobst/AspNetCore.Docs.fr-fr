---
title: Autorisation basée sur les ressources dans ASP.NET Core
author: scottaddie
description: Découvrez comment implémenter l’autorisation basée sur les ressources dans une application ASP.NET Core lorsqu’un attribut Authorize ne suffit.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094280"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="edbd8-103">Autorisation basée sur les ressources dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="edbd8-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="edbd8-104">Stratégie d’autorisation dépend de la ressource sollicitée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="edbd8-105">Envisagez d’un document qui possède une propriété de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="edbd8-105">Consider a document which has an author property.</span></span> <span data-ttu-id="edbd8-106">Seul l’auteur est autorisé à mettre à jour le document.</span><span class="sxs-lookup"><span data-stu-id="edbd8-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="edbd8-107">Par conséquent, le document doit être récupéré à partir du magasin de données avant de l’évaluation de l’autorisation peut se produire.</span><span class="sxs-lookup"><span data-stu-id="edbd8-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="edbd8-108">Évaluation de l’attribut se produit avant la liaison de données et avant l’exécution de l’action qui charge le document ou le Gestionnaire de page.</span><span class="sxs-lookup"><span data-stu-id="edbd8-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="edbd8-109">Pour ces raisons, l’autorisation déclarative avec un `[Authorize]` attribut ne suffit pas.</span><span class="sxs-lookup"><span data-stu-id="edbd8-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="edbd8-110">Au lieu de cela, vous pouvez appeler une méthode d’autorisation personnalisée&mdash;un style appelé impératif d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="edbd8-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="edbd8-111">Utilisez le [exemples d’applications](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) pour Explorer les fonctionnalités décrites dans cette rubrique.</span><span class="sxs-lookup"><span data-stu-id="edbd8-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="edbd8-112">[Créer une application ASP.NET Core et de données utilisateur protégées par autorisation](xref:security/authorization/secure-data) contient un exemple d’application qui utilise l’autorisation basée sur les ressources.</span><span class="sxs-lookup"><span data-stu-id="edbd8-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="edbd8-113">Utiliser l’autorisation impérative</span><span class="sxs-lookup"><span data-stu-id="edbd8-113">Use imperative authorization</span></span>

<span data-ttu-id="edbd8-114">L’autorisation est implémentée comme un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de service et est enregistré dans la collection de service dans la `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="edbd8-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="edbd8-115">Le service est rendu disponible via [injection de dépendance](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) aux gestionnaires de page ou aux actions.</span><span class="sxs-lookup"><span data-stu-id="edbd8-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="edbd8-116">`IAuthorizationService` a deux `AuthorizeAsync` surcharges de méthode : une acceptation de la ressource et le nom de la stratégie et l’autre accepte la ressource et une liste d’exigences à évaluer.</span><span class="sxs-lookup"><span data-stu-id="edbd8-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="edbd8-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="edbd8-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="edbd8-119">Dans l’exemple suivant, la ressource à sécuriser chargée personnalisé `Document` objet.</span><span class="sxs-lookup"><span data-stu-id="edbd8-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="edbd8-120">Un `AuthorizeAsync` surcharge est appelée pour déterminer si l’utilisateur actuel est autorisé à modifier le document fourni.</span><span class="sxs-lookup"><span data-stu-id="edbd8-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="edbd8-121">Une stratégie d’autorisation « EditPolicy » personnalisée est factorisée dans l’arbre de décision.</span><span class="sxs-lookup"><span data-stu-id="edbd8-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="edbd8-122">Consultez [autorisation basée sur des stratégies de personnalisée](xref:security/authorization/policies) pour plus d’informations sur la création de stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="edbd8-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="edbd8-123">Le code suivant exemples supposent que l’authentification a été exécuté et le jeu le `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="edbd8-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="edbd8-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="edbd8-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="edbd8-126">Écrire un gestionnaire de ressources</span><span class="sxs-lookup"><span data-stu-id="edbd8-126">Write a resource-based handler</span></span>

<span data-ttu-id="edbd8-127">Écriture d’un gestionnaire pour l’autorisation basée sur la ressource n’est pas très différente de [écriture d’un gestionnaire d’exigences brut](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="edbd8-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="edbd8-128">Créer une classe de demande personnalisée et implémenter une classe de gestionnaire de condition.</span><span class="sxs-lookup"><span data-stu-id="edbd8-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="edbd8-129">La classe de gestionnaire spécifie l’exigence et le type de ressource.</span><span class="sxs-lookup"><span data-stu-id="edbd8-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="edbd8-130">Par exemple, un gestionnaire utilisant un `SameAuthorRequirement` exigence et un `Document` ressource se présente comme suit :</span><span class="sxs-lookup"><span data-stu-id="edbd8-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="edbd8-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="edbd8-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="edbd8-133">Enregistrer la configuration requise, le gestionnaire dans le `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="edbd8-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="edbd8-134">Spécifications opérationnelles</span><span class="sxs-lookup"><span data-stu-id="edbd8-134">Operational requirements</span></span>

<span data-ttu-id="edbd8-135">Si vous apportez des décisions basées sur les résultats des opérations CRUD (création, lecture, mise à jour, suppression), utilisez la [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe d’assistance.</span><span class="sxs-lookup"><span data-stu-id="edbd8-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="edbd8-136">Cette classe vous permet d’écrire un gestionnaire unique au lieu d’une classe individuelle pour chaque type d’opération.</span><span class="sxs-lookup"><span data-stu-id="edbd8-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="edbd8-137">Pour l’utiliser, fournir des noms d’opération :</span><span class="sxs-lookup"><span data-stu-id="edbd8-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="edbd8-138">Le gestionnaire est implémenté comme suit, à l’aide un `OperationAuthorizationRequirement` exigence et un `Document` ressource :</span><span class="sxs-lookup"><span data-stu-id="edbd8-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="edbd8-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="edbd8-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="edbd8-141">Le gestionnaire précédent valide l’opération à l’aide de la ressource, l’identité d’utilisateur et l’exigence de `Name` propriété.</span><span class="sxs-lookup"><span data-stu-id="edbd8-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="edbd8-142">Pour appeler un gestionnaire de ressources opérationnelles, spécifiez l’opération lors de l’appel `AuthorizeAsync` dans votre gestionnaire de page ou une action.</span><span class="sxs-lookup"><span data-stu-id="edbd8-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="edbd8-143">L’exemple suivant détermine si l’utilisateur authentifié est autorisé à afficher le document fourni.</span><span class="sxs-lookup"><span data-stu-id="edbd8-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="edbd8-144">Le code suivant exemples supposent que l’authentification a été exécuté et le jeu le `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="edbd8-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="edbd8-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="edbd8-146">Si l’autorisation réussit, la page d’affichage du document est retournée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="edbd8-147">Si l’autorisation échoue mais que l’utilisateur est authentifié, retour `ForbidResult` informe tout intergiciel d’authentification qui a l’autorisation a échoué.</span><span class="sxs-lookup"><span data-stu-id="edbd8-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="edbd8-148">A `ChallengeResult` est retourné lorsque l’authentification doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="edbd8-149">Pour les clients de navigateur interactive, il peut être approprié rediriger l’utilisateur vers une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="edbd8-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="edbd8-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="edbd8-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="edbd8-151">Si l’autorisation réussit, la vue du document est retournée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="edbd8-152">Si l’autorisation échoue, retour `ChallengeResult` informe un intergiciel (middleware) d’authentification que l’autorisation a échoué, et l’intergiciel (middleware) peut prendre la réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="edbd8-153">Un code d’état 401 ou 403 peut renvoyer une réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="edbd8-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="edbd8-154">Pour les clients de navigateur interactif, cela peut signifier redirigeant l’utilisateur vers une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="edbd8-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
