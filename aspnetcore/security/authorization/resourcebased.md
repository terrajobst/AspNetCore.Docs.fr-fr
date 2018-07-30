---
title: Autorisation basée sur les ressources dans ASP.NET Core
author: scottaddie
description: Découvrez comment implémenter l’autorisation basée sur les ressources dans une application ASP.NET Core quand un attribut Authorize ne suffira pas.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a110a69c58d5e20a15198378510486daec3d452
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342287"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Autorisation basée sur les ressources dans ASP.NET Core

Stratégie d’autorisation varie selon la ressource sollicitée. Envisagez un document qui possède une propriété de l’auteur. Seul l’auteur est autorisé à mettre à jour le document. Par conséquent, le document doit être récupéré à partir du magasin de données avant de l’évaluation d’autorisation peut se produire.

Évaluation de l’attribut se produit avant la liaison de données et avant l’exécution de l’action qui charge le document ou le Gestionnaire de page. Pour ces raisons, d’autorisation déclaratives avec un `[Authorize]` attribut ne suffira pas. Au lieu de cela, vous pouvez appeler une méthode d’autorisation personnalisée&mdash;un style appelé autorisation impérative.

Utilisez le [exemples d’applications](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([comment télécharger](xref:tutorials/index#how-to-download-a-sample)) pour Explorer les fonctionnalités décrites dans cette rubrique.

[Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation](xref:security/authorization/secure-data) contient un exemple d’application qui utilise l’autorisation basée sur les ressources.

## <a name="use-imperative-authorization"></a>Utiliser l’autorisation impérative

Autorisation est implémentée comme un [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) de service et est enregistré dans la collection de service dans le `Startup` classe. Le service est accessible via [l’injection de dépendances](xref:fundamentals/dependency-injection) aux gestionnaires de page ou des actions.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` a deux `AuthorizeAsync` surcharges de méthode : une acceptation de la ressource et le nom de la stratégie et l’autre acceptant une liste des conditions requises pour évaluer et la ressource.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Dans l’exemple suivant, la ressource à sécuriser est chargée dans un personnalisé `Document` objet. Un `AuthorizeAsync` surcharge est appelée pour déterminer si l’utilisateur actuel est autorisé à modifier le document fourni. Une stratégie d’autorisation « EditPolicy » personnalisée est factorisée dans la décision. Consultez [autorisation basée sur des stratégies de personnalisé](xref:security/authorization/policies) pour plus d’informations sur la création de stratégies d’autorisation.

> [!NOTE]
> Le code suivant exemples supposent que l’authentification s’est exécutée et un ensemble la `User` propriété.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Écrire un gestionnaire de ressources

Écriture d’un gestionnaire pour l’autorisation basée sur les ressources n’est pas très différente de [écriture d’un gestionnaire d’exigences plain](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Créer une classe de condition requise personnalisée et implémenter une classe de gestionnaire d’exigence. La classe de gestionnaire spécifie l’exigence et le type de ressource. Par exemple, un gestionnaire utilisant un `SameAuthorRequirement` exigence et un `Document` ressource se présente comme suit :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Inscrire l’exigence et le gestionnaire dans le `Startup.ConfigureServices` méthode :

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Exigences opérationnelles

Si vous prenez des décisions basées sur les résultats d’opérations CRUD (Create, Read, Update, Delete), utilisez le [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) classe d’assistance. Cette classe vous permet d’écrire un gestionnaire unique au lieu d’une classe individuelle pour chaque type d’opération. Pour l’utiliser, fournir des noms d’opération :

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Le gestionnaire est implémenté comme suit, à l’aide un `OperationAuthorizationRequirement` exigence et un `Document` ressource :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Le gestionnaire précédent valide l’opération à l’aide de la ressource, l’identité d’utilisateur et l’exigence `Name` propriété.

Pour appeler un gestionnaire de ressources opérationnelles, spécifiez l’opération lors de l’appel `AuthorizeAsync` dans votre gestionnaire de page ou une action. L’exemple suivant détermine si l’utilisateur authentifié est autorisé à afficher le document fourni.

> [!NOTE]
> Le code suivant exemples supposent que l’authentification s’est exécutée et un ensemble la `User` propriété.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Si l’autorisation réussit, la page d’affichage du document est retournée. Si l’autorisation échoue, mais l’utilisateur est authentifié, retournant `ForbidResult` informe tout intergiciel d’authentification qui a l’autorisation a échoué. Un `ChallengeResult` est retournée lorsque l’authentification doit être effectuée. Pour les clients de navigateur interactive, il peut être approprié rediriger l’utilisateur vers une page de connexion.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Si l’autorisation réussit, la vue pour le document est retournée. Si l’autorisation échoue, retournant `ChallengeResult` informe tout intergiciel d’authentification que l’autorisation a échoué, et que l’intergiciel (middleware) peut prendre la réponse appropriée. Une réponse appropriée peut retourner un code d’état 401 ou 403. Pour les clients de navigateur interactive, cela peut signifier redirigeant l’utilisateur vers une page de connexion.

---
