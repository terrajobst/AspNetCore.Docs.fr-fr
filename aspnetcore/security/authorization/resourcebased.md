---
title: Autorisation basée sur les ressources dans ASP.NET Core
author: scottaddie
description: Découvrez comment implémenter l’autorisation basée sur les ressources dans une application ASP.NET Core quand un attribut Authorize ne suffira pas.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818367"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="6f36e-103">Autorisation basée sur les ressources dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6f36e-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="6f36e-104">Stratégie d’autorisation varie selon la ressource sollicitée.</span><span class="sxs-lookup"><span data-stu-id="6f36e-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="6f36e-105">Envisagez un document qui possède une propriété de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="6f36e-105">Consider a document that has an author property.</span></span> <span data-ttu-id="6f36e-106">Seul l’auteur est autorisé à mettre à jour le document.</span><span class="sxs-lookup"><span data-stu-id="6f36e-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="6f36e-107">Par conséquent, le document doit être récupéré à partir du magasin de données avant de l’évaluation d’autorisation peut se produire.</span><span class="sxs-lookup"><span data-stu-id="6f36e-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="6f36e-108">Évaluation de l’attribut se produit avant la liaison de données et avant l’exécution du Gestionnaire de page ou de l’action qui charge le document.</span><span class="sxs-lookup"><span data-stu-id="6f36e-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="6f36e-109">Pour ces raisons, d’autorisation déclaratives avec un `[Authorize]` attribut ne suffit pas.</span><span class="sxs-lookup"><span data-stu-id="6f36e-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="6f36e-110">Au lieu de cela, vous pouvez appeler une méthode d’autorisation personnalisée&mdash;un style appelé *autorisation impérative*.</span><span class="sxs-lookup"><span data-stu-id="6f36e-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="6f36e-111">[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6f36e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="6f36e-112">[Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation](xref:security/authorization/secure-data) contient un exemple d’application qui utilise l’autorisation basée sur les ressources.</span><span class="sxs-lookup"><span data-stu-id="6f36e-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="6f36e-113">Utiliser l’autorisation impérative</span><span class="sxs-lookup"><span data-stu-id="6f36e-113">Use imperative authorization</span></span>

<span data-ttu-id="6f36e-114">Autorisation est implémentée comme un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de service et est enregistré dans la collection de service dans le `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="6f36e-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="6f36e-115">Le service est accessible via [l’injection de dépendances](xref:fundamentals/dependency-injection) aux gestionnaires de page ou des actions.</span><span class="sxs-lookup"><span data-stu-id="6f36e-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="6f36e-116">`IAuthorizationService` a deux `AuthorizeAsync` surcharges de méthode : une acceptation de la ressource et le nom de la stratégie et l’autre acceptant une liste des conditions requises pour évaluer et la ressource.</span><span class="sxs-lookup"><span data-stu-id="6f36e-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="6f36e-117">Dans l’exemple suivant, la ressource à sécuriser est chargée dans un personnalisé `Document` objet.</span><span class="sxs-lookup"><span data-stu-id="6f36e-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="6f36e-118">Un `AuthorizeAsync` surcharge est appelée pour déterminer si l’utilisateur actuel est autorisé à modifier le document fourni.</span><span class="sxs-lookup"><span data-stu-id="6f36e-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="6f36e-119">Une stratégie d’autorisation « EditPolicy » personnalisée est factorisée dans la décision.</span><span class="sxs-lookup"><span data-stu-id="6f36e-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="6f36e-120">Consultez [autorisation basée sur des stratégies de personnalisé](xref:security/authorization/policies) pour plus d’informations sur la création de stratégies d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="6f36e-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="6f36e-121">Le code suivant exemples supposent que l’authentification s’est exécutée et un ensemble la `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="6f36e-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="6f36e-122">Écrire un gestionnaire de ressources</span><span class="sxs-lookup"><span data-stu-id="6f36e-122">Write a resource-based handler</span></span>

<span data-ttu-id="6f36e-123">Écriture d’un gestionnaire pour l’autorisation basée sur les ressources n’est pas très différente de [écriture d’un gestionnaire d’exigences plain](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="6f36e-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="6f36e-124">Créer une classe de condition requise personnalisée et implémenter une classe de gestionnaire d’exigence.</span><span class="sxs-lookup"><span data-stu-id="6f36e-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="6f36e-125">Pour plus d’informations sur la création d’une classe d’exigence, consultez [exigences](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="6f36e-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="6f36e-126">La classe de gestionnaire spécifie l’exigence et le type de ressource.</span><span class="sxs-lookup"><span data-stu-id="6f36e-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="6f36e-127">Par exemple, un gestionnaire utilisant un `SameAuthorRequirement` et un `Document` ressource suit :</span><span class="sxs-lookup"><span data-stu-id="6f36e-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="6f36e-128">Dans l’exemple précédent, imaginez que `SameAuthorRequirement` est un cas spécial d’un générique plus `SpecificAuthorRequirement` classe.</span><span class="sxs-lookup"><span data-stu-id="6f36e-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="6f36e-129">Le `SpecificAuthorRequirement` classe (non illustré) contient un `Name` propriété représentant le nom de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="6f36e-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="6f36e-130">Le `Name` propriété peut être définie sur l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="6f36e-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="6f36e-131">Inscrire l’exigence et le gestionnaire dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6f36e-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="6f36e-132">Exigences opérationnelles</span><span class="sxs-lookup"><span data-stu-id="6f36e-132">Operational requirements</span></span>

<span data-ttu-id="6f36e-133">Si vous prenez des décisions basées sur les résultats d’opérations CRUD (Create, Read, Update, Delete), utilisez le [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe d’assistance.</span><span class="sxs-lookup"><span data-stu-id="6f36e-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="6f36e-134">Cette classe vous permet d’écrire un gestionnaire unique au lieu d’une classe individuelle pour chaque type d’opération.</span><span class="sxs-lookup"><span data-stu-id="6f36e-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="6f36e-135">Pour l’utiliser, fournir des noms d’opération :</span><span class="sxs-lookup"><span data-stu-id="6f36e-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="6f36e-136">Le gestionnaire est implémenté comme suit, à l’aide un `OperationAuthorizationRequirement` exigence et un `Document` ressource :</span><span class="sxs-lookup"><span data-stu-id="6f36e-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="6f36e-137">Le gestionnaire précédent valide l’opération à l’aide de la ressource, l’identité d’utilisateur et l’exigence `Name` propriété.</span><span class="sxs-lookup"><span data-stu-id="6f36e-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="6f36e-138">Pour appeler un gestionnaire de ressources opérationnelles, spécifiez l’opération lors de l’appel `AuthorizeAsync` dans votre gestionnaire de page ou une action.</span><span class="sxs-lookup"><span data-stu-id="6f36e-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="6f36e-139">L’exemple suivant détermine si l’utilisateur authentifié est autorisé à afficher le document fourni.</span><span class="sxs-lookup"><span data-stu-id="6f36e-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="6f36e-140">Le code suivant exemples supposent que l’authentification s’est exécutée et un ensemble la `User` propriété.</span><span class="sxs-lookup"><span data-stu-id="6f36e-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="6f36e-141">Si l’autorisation réussit, la page d’affichage du document est retournée.</span><span class="sxs-lookup"><span data-stu-id="6f36e-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="6f36e-142">Si l’autorisation échoue, mais l’utilisateur est authentifié, retournant `ForbidResult` informe tout intergiciel d’authentification qui a l’autorisation a échoué.</span><span class="sxs-lookup"><span data-stu-id="6f36e-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="6f36e-143">Un `ChallengeResult` est retournée lorsque l’authentification doit être effectuée.</span><span class="sxs-lookup"><span data-stu-id="6f36e-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="6f36e-144">Pour les clients de navigateur interactive, il peut être approprié rediriger l’utilisateur vers une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="6f36e-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="6f36e-145">Si l’autorisation réussit, la vue pour le document est retournée.</span><span class="sxs-lookup"><span data-stu-id="6f36e-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="6f36e-146">Si l’autorisation échoue, retournant `ChallengeResult` informe tout intergiciel d’authentification que l’autorisation a échoué, et que l’intergiciel (middleware) peut prendre la réponse appropriée.</span><span class="sxs-lookup"><span data-stu-id="6f36e-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="6f36e-147">Une réponse appropriée peut retourner un code d’état 401 ou 403.</span><span class="sxs-lookup"><span data-stu-id="6f36e-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="6f36e-148">Pour les clients de navigateur interactive, cela peut signifier redirigeant l’utilisateur vers une page de connexion.</span><span class="sxs-lookup"><span data-stu-id="6f36e-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
