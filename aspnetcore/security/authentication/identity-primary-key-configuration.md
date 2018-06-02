---
title: Configurer le type de données de clé primaire d’identité dans ASP.NET Core
author: AdrienTorris
description: En savoir plus sur les étapes de configuration du type de données utilisé pour la clé primaire ASP.NET Core Identity.
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 49d5ef94abeb5bd616c5ddbcdd4358a58a8e63a4
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Configurer le type de données de clé primaire d’identité dans ASP.NET Core

Identity de ASP.NET Core permet de configurer le type de données utilisé pour représenter une clé primaire. Identity utilise le type de données `string`  par défaut. Vous pouvez substituer ce comportement. 

## <a name="customize-the-primary-key-data-type"></a>Personnaliser le type de données de clé primaire

1. Créer une implémentation personnalisée de la classe [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1). Il représente le type à utiliser pour la création d’objets utilisateur. Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. Créer une implémentation personnalisée de la classe [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1). Il représente le type à utiliser pour créer des objets de rôle. Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. Créer une classe de contexte de base de données personnalisée. Il hérite de la classe de contexte de base de données Entity Framework utilisée pour l’identité. Les arguments `TUser` et `TRole` référencent respectivement les classes d’utilisateur et le rôle personnalisés créés à l’étape précédente. Le `Guid` type de données est défini pour la clé primaire.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans la classe de démarrage de l’application.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   Le `AddEntityFrameworkStores` méthode n’accepte pas un `TKey` argument comme il le faisiez dans ASP.NET Core 1.x. Le type de données de la clé primaire est déduit en analysant l'objet `DbContext`.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   La méthode `AddEntityFrameworkStores` accepte un argument `TKey` indiquant le type de données de la clé primaire.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a>Tester les modifications

Une fois les modifications de configuration effectuées, la propriété qui représente la clé primaire reflète le nouveau type de données. L’exemple suivant montre comment accéder à la propriété dans un contrôleur MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
