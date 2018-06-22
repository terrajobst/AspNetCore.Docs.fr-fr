---
title: Configurer le type de données de clé primaire d’identité dans ASP.NET Core
author: AdrienTorris
description: En savoir plus sur les étapes de configuration du type de données utilisé pour la clé primaire ASP.NET Core Identity.
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274354"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="3d434-103">Configurer le type de données de clé primaire d’identité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d434-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="3d434-104">Identity de ASP.NET Core permet de configurer le type de données utilisé pour représenter une clé primaire.</span><span class="sxs-lookup"><span data-stu-id="3d434-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="3d434-105">Identity utilise le type de données `string`  par défaut.</span><span class="sxs-lookup"><span data-stu-id="3d434-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="3d434-106">Vous pouvez substituer ce comportement. </span><span class="sxs-lookup"><span data-stu-id="3d434-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="3d434-107">Personnaliser le type de données de clé primaire</span><span class="sxs-lookup"><span data-stu-id="3d434-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="3d434-108">Créer une implémentation personnalisée de la classe [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1).</span><span class="sxs-lookup"><span data-stu-id="3d434-108">Create a custom implementation of the [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="3d434-109">Il représente le type à utiliser pour la création d’objets utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3d434-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="3d434-110">Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.</span><span class="sxs-lookup"><span data-stu-id="3d434-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="3d434-111">Créer une implémentation personnalisée de la classe [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1).</span><span class="sxs-lookup"><span data-stu-id="3d434-111">Create a custom implementation of the [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="3d434-112">Il représente le type à utiliser pour créer des objets de rôle.</span><span class="sxs-lookup"><span data-stu-id="3d434-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="3d434-113">Dans l’exemple suivant, la valeur par défaut `string` type est remplacé par `Guid`.</span><span class="sxs-lookup"><span data-stu-id="3d434-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="3d434-114">Créer une classe de contexte de base de données personnalisée.</span><span class="sxs-lookup"><span data-stu-id="3d434-114">Create a custom database context class.</span></span> <span data-ttu-id="3d434-115">Il hérite de la classe de contexte de base de données Entity Framework utilisée pour l’identité.</span><span class="sxs-lookup"><span data-stu-id="3d434-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="3d434-116">Les arguments `TUser` et `TRole` référencent respectivement les classes d’utilisateur et le rôle personnalisés créés à l’étape précédente.</span><span class="sxs-lookup"><span data-stu-id="3d434-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="3d434-117">Le `Guid` type de données est défini pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="3d434-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="3d434-118">Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans la classe de démarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="3d434-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3d434-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3d434-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   <span data-ttu-id="3d434-120">Le `AddEntityFrameworkStores` méthode n’accepte pas un `TKey` argument comme il le faisiez dans ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="3d434-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="3d434-121">Le type de données de la clé primaire est déduit en analysant l'objet `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3d434-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3d434-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3d434-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   <span data-ttu-id="3d434-123">La méthode `AddEntityFrameworkStores` accepte un argument `TKey` indiquant le type de données de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="3d434-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a><span data-ttu-id="3d434-124">Tester les modifications</span><span class="sxs-lookup"><span data-stu-id="3d434-124">Test the changes</span></span>

<span data-ttu-id="3d434-125">Une fois les modifications de configuration effectuées, la propriété qui représente la clé primaire reflète le nouveau type de données.</span><span class="sxs-lookup"><span data-stu-id="3d434-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="3d434-126">L’exemple suivant montre comment accéder à la propriété dans un contrôleur MVC.</span><span class="sxs-lookup"><span data-stu-id="3d434-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
