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
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorisation basée sur des stratégies dans ASP.NET Core

Dans les coulisses, [autorisation basée sur le rôle](xref:security/authorization/roles) et [autorisation basée sur les revendications](xref:security/authorization/claims) utilisent une exigence, un gestionnaire de condition et une stratégie préconfigurée. Ces blocs de construction prend en charge l’expression d’évaluations d’autorisation dans le code. Le résultat est une structure d’autorisation plus riches, réutilisables et faciles à tester.

Une stratégie d’autorisation se compose d’une ou plusieurs exigences. Il est inscrit dans le cadre de la configuration du service d’autorisation, dans le `Startup.ConfigureServices` méthode :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

Dans l’exemple précédent, une stratégie de « AtLeast21 » est créée. Il a une seule exigence&mdash;celle d’un minimum d’ancienneté, qui est fourni en tant que paramètre à cette exigence.

Les stratégies sont appliquées à l’aide de la `[Authorize]` attribut avec le nom de la stratégie. Exemple :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Spécifications

Une condition d’autorisation est une collection de paramètres de données une stratégie peut utiliser pour évaluer le principal utilisateur actuel. Dans notre stratégie de « AtLeast21 », l’exigence est un paramètre unique&mdash;l’ancienneté minimale. Implémente une exigence [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), qui est une interface de marqueur vide. Une exigence paramétrable ancienneté minimale peut être implémentée comme suit :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Une exigence n’a pas besoin d’avoir des propriétés ou des données.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestionnaires d’autorisation

Un gestionnaire d’autorisation est responsable de l’évaluation des propriétés d’une exigence. Le Gestionnaire d’autorisation évalue la configuration requise par rapport à un fourni [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) pour déterminer si l’accès est autorisé.

Une condition peut avoir [plusieurs gestionnaires](#security-authorization-policies-based-multiple-handlers). Un gestionnaire peut hériter [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), où `TRequirement` est la nécessité d’être gérée. Vous pouvez également, peut implémenter un gestionnaire [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pour gérer plusieurs types de spécifications.

### <a name="use-a-handler-for-one-requirement"></a>Utiliser un gestionnaire pour une exigence

<a name="security-authorization-handler-example"></a>

Voici un exemple d’une relation un à un dans lequel un gestionnaire de l’ancienneté minimale utilise une seule exigence :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Le code précédent détermine si l’utilisateur principal a une date de naissance de revendication qui a été émis par un émetteur connu et approuvé. L’autorisation ne peut pas se produire lors de la revendication est manquante, auquel cas une tâche achevée est retournée. Lorsqu’une revendication est présente, l’âge de l’utilisateur est calculée. Si l’utilisateur remplit l’ancienneté minimale définie par la configuration requise, l’autorisation est considérée comme réussie. Lors de l’autorisation est accordée, `context.Succeed` est appelé avec l’exigence satisfait par son seul paramètre.

### <a name="use-a-handler-for-multiple-requirements"></a>Utiliser un gestionnaire pour plusieurs conditions

Voici un exemple d’une relation un-à-plusieurs dans lequel un gestionnaire d’autorisation permettre gérer trois différents types de conditions :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Le code précédent traverse [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;une propriété contenant des exigences ne pas marquée comme ayant réussie. Pour un `ReadPermission` exigence, l’utilisateur doit être un propriétaire ou un commanditaire pour accéder à la ressource demandée. Dans le cas d’un `EditPermission` ou `DeletePermission` exigence, il doit être un propriétaire à accéder à la ressource demandée.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Inscription du Gestionnaire

Gestionnaires sont enregistrés dans la collection de services lors de la configuration. Exemple :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Chaque gestionnaire est ajouté à la collection de services en appelant `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Ce qui doit retourner un gestionnaire ?

Notez que le `Handle` méthode dans le [handler, exemple](#security-authorization-handler-example) ne retourne aucune valeur. Comment est un état de réussite ou échec indiqué ?

* Un gestionnaire indique la réussite en appelant `context.Succeed(IAuthorizationRequirement requirement)`, en passant l’exigence qui a été validé avec succès.

* Un gestionnaire n’a pas besoin de gérer les défaillances en règle générale, comme les autres gestionnaires pour les mêmes exigences peuvent réussir.

* Pour garantir la défaillance, même si d’autres gestionnaires d’exigences réussissent, appelez `context.Fail`.

Lorsque la valeur `false`, le [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) propriété (disponible dans ASP.NET Core 1.1 et versions ultérieure) court-circuite l’exécution des gestionnaires lorsque `context.Fail` est appelée. `InvokeHandlersAfterFailure` valeur par défaut est `true`, auquel cas tous les gestionnaires sont appelés. Ainsi, les conditions requises pour produire des effets secondaires, tels que la journalisation, qui ont toujours lieu même si `context.Fail` a été appelée dans un autre gestionnaire.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Pourquoi devrais-je plusieurs gestionnaires pour une spécification ?

Dans les cas où vous souhaitez d’évaluation doivent se trouver sur un **ou** base, implémenter plusieurs gestionnaires pour une spécification unique. Par exemple, Microsoft a les portes n’ouvrir avec des cartes de clé. Si vous laissez votre carte de clé à la maison, la réceptionniste imprime un autocollant temporaire et ouvre la porte pour vous. Dans ce scénario, il vous aurait une seule exigence, *BuildingEntry*, mais plusieurs gestionnaires, chacun d'entre eux examinant une seule condition.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assurez-vous que les deux gestionnaires sont [inscrit](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Si un gestionnaire réussit lorsqu’une stratégie évalue la `BuildingEntryRequirement`, réussit l’évaluation de stratégie.

## <a name="using-a-func-to-fulfill-a-policy"></a>À l’aide d’une variable func pour répondre à une stratégie

Il peut arriver dans quel remplissant une stratégie est simple à exprimer dans le code. Il est possible de fournir un `Func<AuthorizationHandlerContext, bool>` lorsque vous configurez votre stratégie avec le `RequireAssertion` Générateur de stratégie.

Par exemple, le précédent `BadgeEntryHandler` peut être réécrit comme suit :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accès au contexte de demande MVC dans les gestionnaires

Le `HandleRequirementAsync` méthode que vous implémentez dans un gestionnaire d’autorisation a deux paramètres : un `AuthorizationHandlerContext` et `TRequirement` que vous gérez. Infrastructures telles que MVC ou Jabbr sont libres d’ajouter n’importe quel objet à la `Resource` propriété sur le `AuthorizationHandlerContext` pour passer des informations supplémentaires.

Par exemple, MVC passe une instance de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) dans le `Resource` propriété. Cette propriété fournit l’accès aux `HttpContext`, `RouteData`et tout autre fournie par MVC et les Pages Razor.

L’utilisation de la `Resource` propriété est framework spécifique. À l’aide des informations contenues dans le `Resource` propriété limite vos stratégies d’autorisation aux infrastructures particuliers. Vous devez effectuer un cast du `Resource` à l’aide de la propriété le `as` mot clé, puis confirmez la conversion a été pour vérifier votre code ne se bloque avec une `InvalidCastException` lorsque vous travaillez sur d’autres infrastructures :

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
