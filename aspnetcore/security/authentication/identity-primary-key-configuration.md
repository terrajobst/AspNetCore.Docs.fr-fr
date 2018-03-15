---
title: "Configurer le type de données de clé primaire d’identité dans ASP.NET Core"
author: AdrienTorris
description: "En savoir plus sur les étapes de configuration du type de données utilisé pour la clé primaire ASP.NET Core Identity."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 02482b81faa64b01765a90c2c6ffe9cf92b1a7e7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a>Configurer le type de données de clé primaire d’identité dans ASP.NET Core

Identité de ASP.NET Core permet de configurer le type de données utilisé pour représenter une clé primaire. Identité utilise le `string` type de données par défaut. Vous pouvez substituer ce comportement.

## <a name="customize-the-primary-key-data-type"></a>Personnaliser le type de données de clé primaire

1. Créer une implémentation personnalisée de la [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) classe. Il représente le type à utiliser pour la création d’objets utilisateur. Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Créer une implémentation personnalisée de la [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) classe. Il représente le type à utiliser pour créer des objets de rôle. Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Créer une classe de contexte de base de données personnalisée. Il hérite de la classe de contexte de base de données Entity Framework utilisée pour l’identité. Le `TUser` et `TRole` arguments de référencent aux classes d’utilisateur et le rôle personnalisés créés à l’étape précédente, respectivement. Le `Guid` type de données est défini pour la clé primaire.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans la classe de démarrage de l’application.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    Le `AddEntityFrameworkStores` méthode n’accepte pas un `TKey` argument comme il le faisiez dans ASP.NET Core 1.x. Type de données de la clé primaire est déduit en analysant le `DbContext` objet.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    Le `AddEntityFrameworkStores` méthode accepte un `TKey` argument indiquant le type de données de la clé primaire.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Tester les modifications

Une fois les modifications de configuration, la propriété qui représente la clé primaire reflète le nouveau type de données. L’exemple suivant montre comment accéder à la propriété dans un contrôleur MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
