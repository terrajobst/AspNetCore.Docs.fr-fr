---
title: Autorisation basée sur des stratégies dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer et utiliser des gestionnaires de stratégie d’autorisation pour mettre en œuvre les spécifications d’autorisation dans une application ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072859"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorisation basée sur des stratégies dans ASP.NET Core

Dans les coulisses, [d’autorisation basée sur le rôle](xref:security/authorization/roles) et [d’autorisation basée sur les revendications](xref:security/authorization/claims) utilisent une spécification, un gestionnaire de condition et une stratégie préconfigurée. Ces blocs de construction prend en charge l’expression d’évaluations d’autorisation dans le code. Le résultat est une structure d’autorisation plus riches, réutilisables, testable.

Une stratégie d’autorisation se compose d’une ou plusieurs conditions. Il est inscrit dans le cadre de la configuration du service d’autorisation, de la `Startup.ConfigureServices` méthode :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

Dans l’exemple précédent, une stratégie de « AtLeast21 » est créée. Il a une seule exigence&mdash;celle d’un minimum d’ancienneté, qui est fournie en tant que paramètre à la spécification.

Les stratégies sont appliquées à l’aide de la `[Authorize]` attribut avec le nom de la stratégie. Par exemple :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Spécifications

Une demande d’autorisation est une collection de paramètres de données une stratégie peut utiliser pour évaluer le principal utilisateur actuel. Dans notre stratégie « AtLeast21 », la spécification est un paramètre unique&mdash;l’ancienneté minimale. Une exigence implémente [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), qui est une interface de marqueur vide. Une spécification de l’ancienneté minimale paramétrable peut être implémentée comme suit :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Une exigence n’a pas besoin d’avoir des données ou des propriétés.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Gestionnaires d’autorisation

Un gestionnaire d’autorisation est responsable de l’évaluation des propriétés d’une spécification. Le Gestionnaire d’autorisation évalue la configuration requise par rapport à un [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) pour déterminer si l’accès est autorisé.

Une condition peut avoir [plusieurs gestionnaires](#security-authorization-policies-based-multiple-handlers). Un gestionnaire peut hériter [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), où `TRequirement` est la nécessité d’être géré. Vous pouvez également peut implémenter un gestionnaire [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pour gérer plusieurs types de spécifications.

### <a name="use-a-handler-for-one-requirement"></a>Utiliser un gestionnaire pour une exigence

<a name="security-authorization-handler-example"></a>

Voici un exemple de type de relation dans laquelle un gestionnaire d’âge minimal utilise une seule exigence :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Le code précédent détermine si l’utilisateur principal a une date de naissance de revendications qui a été émis par un émetteur connu et approuvé. L’autorisation ne peut pas se produire lors de la revendication est manquante, auquel cas une tâche terminée est renvoyée. Lorsqu’une revendication est présente, l’âge de l’utilisateur est calculée. Si l’utilisateur répond à la durée de vie minimale définie par la spécification, l’autorisation est considérée comme réussie. Lorsqu’une autorisation est réussie, `context.Succeed` est appelé avec la spécification satisfaite comme seul paramètre.

### <a name="use-a-handler-for-multiple-requirements"></a>Utiliser un gestionnaire pour plusieurs conditions

Voici un exemple d’une relation un-à-plusieurs dans laquelle un gestionnaire d’autorisation utilise trois conditions :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Le code précédent traverse [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;une propriété contenant des spécifications ne pas marquée comme réussie. Si l’utilisateur a accès en lecture, il doit être un propriétaire ou un partenaire à accéder à la ressource demandée. Si l’utilisateur a modifier ou supprimer des autorisations, il doit être un propriétaire pour accéder à la ressource demandée. Lorsqu’une autorisation est réussie, `context.Succeed` est appelé avec la spécification satisfaite comme seul paramètre.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Inscription du Gestionnaire

Les gestionnaires sont enregistrés dans la collection de services lors de la configuration. Par exemple :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Chaque gestionnaire est ajouté à la collection de services en appelant `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Ce qui doit retourner un gestionnaire ?

Notez que la `Handle` méthode dans le [exemple de gestionnaire](#security-authorization-handler-example) ne retourne aucune valeur. Comment est un état de réussite ou échec indiqué ?

* Un gestionnaire indique la réussite en appelant `context.Succeed(IAuthorizationRequirement requirement)`, en passant à la demande qui a été validé.

* Un gestionnaire n’a pas besoin de gérer les défaillances en règle générale, comme les autres gestionnaires pour la même exigence peuvent réussir.

* Pour garantir la défaillance, même si d’autres gestionnaires de spécification réussissent, appelez `context.Fail`.

Lorsque la valeur `false`, le [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) , propriété (disponible dans ASP.NET Core 1.1 et ultérieure) court-circuite l’exécution des gestionnaires lorsque `context.Fail` est appelée. `InvokeHandlersAfterFailure` valeur par défaut est `true`, auquel cas tous les gestionnaires sont appelés. Cela permet la configuration requise produire des effets secondaires, tels que la journalisation, qui ont toujours lieu même si `context.Fail` a été appelée dans un autre gestionnaire.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Pourquoi voudrais-je plusieurs gestionnaires pour une spécification ?

Dans le cas où vous souhaitez d’évaluation sur une **ou** à la fois, implémenter plusieurs gestionnaires pour une spécification unique. Par exemple, Microsoft a portes ouvre uniquement avec les cartes de clé. Si vous laissez votre carte de clé à la maison, la réceptionniste imprime un autocollant temporaire et ouvre la porte pour vous. Dans ce scénario, vous aurait une seule exigence, *BuildingEntry*, mais plusieurs gestionnaires, chacun d'entre eux examinant une seule exigence.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Assurez-vous que les deux gestionnaires sont [inscrit](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Si un gestionnaire réussit lorsqu’une stratégie évalue la `BuildingEntryRequirement`, l’évaluation de stratégie a réussi.

## <a name="using-a-func-to-fulfill-a-policy"></a>À l’aide d’une func pour répondre à une stratégie

Il peut arriver en les remplissant une stratégie est simple pour exprimer dans le code. Il est possible de fournir un `Func<AuthorizationHandlerContext, bool>` lors de la configuration de votre stratégie avec le `RequireAssertion` le Générateur de stratégie.

Par exemple, le précédent `BadgeEntryHandler` peut être réécrit comme suit :

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Accès au contexte de demande MVC dans les gestionnaires

Le `HandleRequirementAsync` méthode que vous implémentez dans un gestionnaire d’autorisation possède deux paramètres : un `AuthorizationHandlerContext` et `TRequirement` vous gérez. Infrastructures telles que MVC ou Jabbr sont libres d’ajouter n’importe quel objet pour le `Resource` propriété sur le `AuthorizationHandlerContext` pour passer des informations supplémentaires.

Par exemple, MVC passe une instance de [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) dans le `Resource` propriété. Cette propriété fournit l’accès aux `HttpContext`, `RouteData`et tout autre fournie par MVC et les Pages Razor.

L’utilisation de la `Resource` propriété est spécifiques de l’infrastructure. À l’aide des informations contenues dans le `Resource` propriété limite vos stratégies d’autorisation pour les infrastructures particulières. Vous devez effectuer un cast du `Resource` à l’aide de la propriété le `as` (mot clé), puis confirmez le cast a réussir pour vérifier votre code ne se bloquer avec une `InvalidCastException` sur les autres infrastructures :

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
