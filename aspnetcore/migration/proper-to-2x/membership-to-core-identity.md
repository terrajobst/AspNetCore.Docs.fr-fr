---
title: Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0
author: isaac2004
description: Découvrez comment migrer des applications ASP.NET existantes à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3b708da13ff9f2887eee87ea17844312a4fe1b8d
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65084878"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="d16b9-103">Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="d16b9-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="d16b9-104">De [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="d16b9-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="d16b9-105">Cet article illustre la migration du schéma de base de données pour les applications ASP.NET à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="d16b9-106">Ce document fournit les étapes nécessaires pour migrer le schéma de base de données pour les applications basées sur l’appartenance ASP.NET vers le schéma de base de données utilisé pour ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="d16b9-107">Pour plus d’informations sur la migration à partir de l’authentification basée sur l’appartenance ASP.NET vers ASP.NET Identity, consultez [migrer une application existante à partir de l’appartenance SQL vers ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="d16b9-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="d16b9-108">Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity dans ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="d16b9-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="d16b9-109">Révision de schéma d’appartenance</span><span class="sxs-lookup"><span data-stu-id="d16b9-109">Review of Membership schema</span></span>

<span data-ttu-id="d16b9-110">Avant ASP.NET 2.0, les développeurs ont été chargés de créer l’ensemble du processus d’authentification et d’autorisation pour leurs applications.</span><span class="sxs-lookup"><span data-stu-id="d16b9-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="d16b9-111">Avec ASP.NET 2.0, l’appartenance a été introduite, fournissant une solution réutilisable pour la gestion de la sécurité dans les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d16b9-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="d16b9-112">Les développeurs ont désormais en mesure d’amorçage d’un schéma dans une base de données SQL Server avec le [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) commande.</span><span class="sxs-lookup"><span data-stu-id="d16b9-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="d16b9-113">Après avoir exécuté cette commande, les tables suivantes ont été créées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="d16b9-113">After running this command, the following tables were created in the database.</span></span>

  ![Tables d’appartenances](identity/_static/membership-tables.png)

<span data-ttu-id="d16b9-115">Pour migrer des applications existantes vers ASP.NET Core 2.0 Identity, les données dans ces tables doivent être migrés vers les tables utilisées par le nouveau schéma d’identité.</span><span class="sxs-lookup"><span data-stu-id="d16b9-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="d16b9-116">ASP.NET Core Identity 2.0 schéma</span><span class="sxs-lookup"><span data-stu-id="d16b9-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="d16b9-117">ASP.NET Core 2.0 suit le [identité](/aspnet/identity/index) principe introduit dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="d16b9-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="d16b9-118">Bien que le principe est partagé, l’implémentation entre les infrastructures est différente, même entre les versions d’ASP.NET Core (consultez [migrer d’authentification et identité vers ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="d16b9-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="d16b9-119">Le moyen le plus rapide pour afficher le schéma pour ASP.NET Core 2.0 Identity consiste à créer une application ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d16b9-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="d16b9-120">Suivez ces étapes dans Visual Studio 2017 :</span><span class="sxs-lookup"><span data-stu-id="d16b9-120">Follow these steps in Visual Studio 2017:</span></span>

1. <span data-ttu-id="d16b9-121">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="d16b9-121">Select **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="d16b9-122">Créer un nouveau **Application Web ASP.NET Core** projet nommé *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-122">Create a new **ASP.NET Core Web Application** project named *CoreIdentitySample*.</span></span>
1. <span data-ttu-id="d16b9-123">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Web Application**.</span><span class="sxs-lookup"><span data-stu-id="d16b9-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="d16b9-124">Ce modèle génère un [Pages Razor](xref:razor-pages/index) application.</span><span class="sxs-lookup"><span data-stu-id="d16b9-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="d16b9-125">Avant de cliquer sur **OK**, cliquez sur **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="d16b9-125">Before clicking **OK**, click **Change Authentication**.</span></span>
1. <span data-ttu-id="d16b9-126">Choisissez **comptes d’utilisateur individuels** pour les modèles d’identité.</span><span class="sxs-lookup"><span data-stu-id="d16b9-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="d16b9-127">Enfin, cliquez sur **OK**, puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="d16b9-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="d16b9-128">Visual Studio crée un projet à l’aide du modèle ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>
1. <span data-ttu-id="d16b9-129">Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package** pour ouvrir le **Console du Gestionnaire de Package** Fenêtre (PMC).</span><span class="sxs-lookup"><span data-stu-id="d16b9-129">Select **Tools** > **NuGet Package Manager** > **Package Manager Console** to open the **Package Manager Console** (PMC) window.</span></span>
1. <span data-ttu-id="d16b9-130">Accédez à la racine du projet dans PMC et exécutez le [Entity Framework (EF) Core](/ef/core) `Update-Database` commande.</span><span class="sxs-lookup"><span data-stu-id="d16b9-130">Navigate to the project root in PMC, and run the [Entity Framework (EF) Core](/ef/core) `Update-Database` command.</span></span>

    <span data-ttu-id="d16b9-131">Identité ASP.NET Core 2.0 utilise EF Core pour interagir avec la base de données stockant les données d’authentification.</span><span class="sxs-lookup"><span data-stu-id="d16b9-131">ASP.NET Core 2.0 Identity uses EF Core to interact with the database storing the authentication data.</span></span> <span data-ttu-id="d16b9-132">Afin que l’application nouvellement créée fonctionne, il doit être une base de données pour stocker ces données.</span><span class="sxs-lookup"><span data-stu-id="d16b9-132">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="d16b9-133">Après avoir créé une nouvelle application, le moyen le plus rapide pour inspecter le schéma dans un environnement de base de données consiste à créer la base de données à l’aide [Migrations d’EF Core](/ef/core/managing-schemas/migrations/).</span><span class="sxs-lookup"><span data-stu-id="d16b9-133">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using [EF Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="d16b9-134">Ce processus crée une base de données, en local ou un autre emplacement, qui reproduit ce schéma.</span><span class="sxs-lookup"><span data-stu-id="d16b9-134">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="d16b9-135">Passez en revue la documentation précédente pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d16b9-135">Review the preceding documentation for more information.</span></span>

    <span data-ttu-id="d16b9-136">Commandes EF Core utilisent la chaîne de connexion pour la base de données spécifiée dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-136">EF Core commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="d16b9-137">La chaîne de connexion cible d’une base de données sur *localhost* nommé *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="d16b9-138">Dans ce paramètre, EF Core est configuré pour utiliser le `DefaultConnection` chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="d16b9-138">In this setting, EF Core is configured to use the `DefaultConnection` connection string.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```

1. <span data-ttu-id="d16b9-139">Sélectionnez **vue** > **Explorateur d’objets SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d16b9-139">Select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="d16b9-140">Développez le nœud correspondant au nom de la base de données spécifié dans le `ConnectionStrings:DefaultConnection` propriété du *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-140">Expand the node corresponding to the database name specified in the `ConnectionStrings:DefaultConnection` property of *appsettings.json*.</span></span>

    <span data-ttu-id="d16b9-141">Le `Update-Database` commande a créé la base de données spécifié avec le schéma et toutes les données requises pour l’initialisation d’application.</span><span class="sxs-lookup"><span data-stu-id="d16b9-141">The `Update-Database` command created the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="d16b9-142">L’image suivante illustre la structure de table est créée avec les étapes précédentes.</span><span class="sxs-lookup"><span data-stu-id="d16b9-142">The following image depicts the table structure that's created with the preceding steps.</span></span>

    ![Tables d’identité](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="d16b9-144">Migrer le schéma</span><span class="sxs-lookup"><span data-stu-id="d16b9-144">Migrate the schema</span></span>

<span data-ttu-id="d16b9-145">Il existe des différences subtiles dans les structures de table et les champs pour l’appartenance et ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-145">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="d16b9-146">Le modèle a des modifications substantielles pour l’authentification/autorisation avec les applications ASP.NET et ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d16b9-146">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="d16b9-147">Les objets clés sont toujours utilisés avec l’identité sont *utilisateurs* et *rôles*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-147">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="d16b9-148">Voici les tables de mappage pour *utilisateurs*, *rôles*, et *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-148">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="d16b9-149">Utilisateurs</span><span class="sxs-lookup"><span data-stu-id="d16b9-149">Users</span></span>

|<span data-ttu-id="d16b9-150">*Identity<br>(dbo.AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-150">*Identity<br>(dbo.AspNetUsers)*</span></span>        ||<span data-ttu-id="d16b9-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-151">*Membership<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*</span></span>||
|----------------------------------------|-----------------------------------------------------------|
|<span data-ttu-id="d16b9-152">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-152">**Field Name**</span></span>                 |<span data-ttu-id="d16b9-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-153">**Type**</span></span>|<span data-ttu-id="d16b9-154">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-154">**Field Name**</span></span>                                    |<span data-ttu-id="d16b9-155">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-155">**Type**</span></span>|
|`Id`                           |<span data-ttu-id="d16b9-156">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-156">string</span></span>  |`aspnet_Users.UserId`                             |<span data-ttu-id="d16b9-157">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-157">string</span></span>  |
|`UserName`                     |<span data-ttu-id="d16b9-158">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-158">string</span></span>  |`aspnet_Users.UserName`                           |<span data-ttu-id="d16b9-159">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-159">string</span></span>  |
|`Email`                        |<span data-ttu-id="d16b9-160">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-160">string</span></span>  |`aspnet_Membership.Email`                         |<span data-ttu-id="d16b9-161">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-161">string</span></span>  |
|`NormalizedUserName`           |<span data-ttu-id="d16b9-162">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-162">string</span></span>  |`aspnet_Users.LoweredUserName`                    |<span data-ttu-id="d16b9-163">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-163">string</span></span>  |
|`NormalizedEmail`              |<span data-ttu-id="d16b9-164">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-164">string</span></span>  |`aspnet_Membership.LoweredEmail`                  |<span data-ttu-id="d16b9-165">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-165">string</span></span>  |
|`PhoneNumber`                  |<span data-ttu-id="d16b9-166">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-166">string</span></span>  |`aspnet_Users.MobileAlias`                        |<span data-ttu-id="d16b9-167">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-167">string</span></span>  |
|`LockoutEnabled`               |<span data-ttu-id="d16b9-168">bit</span><span class="sxs-lookup"><span data-stu-id="d16b9-168">bit</span></span>     |`aspnet_Membership.IsLockedOut`                   |<span data-ttu-id="d16b9-169">bit</span><span class="sxs-lookup"><span data-stu-id="d16b9-169">bit</span></span>     |

> [!NOTE]
> <span data-ttu-id="d16b9-170">Pas tous les mappages de champs ressemblent à des relations un à un à partir de l’appartenance à ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-170">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="d16b9-171">Le tableau précédent prend le schéma utilisateur d’appartenance par défaut et le mappe au schéma ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-171">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="d16b9-172">Tous les autres champs personnalisés qui ont été utilisés pour l’appartenance doivent être mappées manuellement.</span><span class="sxs-lookup"><span data-stu-id="d16b9-172">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="d16b9-173">Dans ce mappage, il n’existe aucun mappage pour les mots de passe, comme critères de mot de passe et le mot de passe sels ne migrent entre les deux.</span><span class="sxs-lookup"><span data-stu-id="d16b9-173">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="d16b9-174">**Il est recommandé de laisser le mot de passe avec la valeur null et demander aux utilisateurs de réinitialiser leurs mots de passe.**</span><span class="sxs-lookup"><span data-stu-id="d16b9-174">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="d16b9-175">Dans ASP.NET Core Identity, `LockoutEnd` doit être définie par une date à l’avenir, si l’utilisateur est verrouillé. Ceci est illustré dans le script de migration.</span><span class="sxs-lookup"><span data-stu-id="d16b9-175">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="d16b9-176">Rôles</span><span class="sxs-lookup"><span data-stu-id="d16b9-176">Roles</span></span>

|<span data-ttu-id="d16b9-177">*Identity<br>(dbo.AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-177">*Identity<br>(dbo.AspNetRoles)*</span></span>        ||<span data-ttu-id="d16b9-178">*Membership<br>(dbo.aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-178">*Membership<br>(dbo.aspnet_Roles)*</span></span>||
|----------------------------------------|-----------------------------------|
|<span data-ttu-id="d16b9-179">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-179">**Field Name**</span></span>                 |<span data-ttu-id="d16b9-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-180">**Type**</span></span>|<span data-ttu-id="d16b9-181">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-181">**Field Name**</span></span>   |<span data-ttu-id="d16b9-182">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-182">**Type**</span></span>         |
|`Id`                           |<span data-ttu-id="d16b9-183">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-183">string</span></span>  |`RoleId`         | <span data-ttu-id="d16b9-184">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-184">string</span></span>          |
|`Name`                         |<span data-ttu-id="d16b9-185">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-185">string</span></span>  |`RoleName`       | <span data-ttu-id="d16b9-186">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-186">string</span></span>          |
|`NormalizedName`               |<span data-ttu-id="d16b9-187">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-187">string</span></span>  |`LoweredRoleName`| <span data-ttu-id="d16b9-188">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-188">string</span></span>          |

### <a name="user-roles"></a><span data-ttu-id="d16b9-189">Rôles d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="d16b9-189">User Roles</span></span>

|<span data-ttu-id="d16b9-190">*Identity<br>(dbo.AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-190">*Identity<br>(dbo.AspNetUserRoles)*</span></span>||<span data-ttu-id="d16b9-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="d16b9-191">*Membership<br>(dbo.aspnet_UsersInRoles)*</span></span>||
|------------------------------------|------------------------------------------|
|<span data-ttu-id="d16b9-192">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-192">**Field Name**</span></span>           |<span data-ttu-id="d16b9-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-193">**Type**</span></span>  |<span data-ttu-id="d16b9-194">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="d16b9-194">**Field Name**</span></span>|<span data-ttu-id="d16b9-195">**Type**</span><span class="sxs-lookup"><span data-stu-id="d16b9-195">**Type**</span></span>                   |
|`RoleId`                 |<span data-ttu-id="d16b9-196">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-196">string</span></span>    |`RoleId`      |<span data-ttu-id="d16b9-197">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-197">string</span></span>                     |
|`UserId`                 |<span data-ttu-id="d16b9-198">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-198">string</span></span>    |`UserId`      |<span data-ttu-id="d16b9-199">string</span><span class="sxs-lookup"><span data-stu-id="d16b9-199">string</span></span>                     |

<span data-ttu-id="d16b9-200">Référencez les tables de mappage précédent lors de la création d’un script de migration pour *utilisateurs* et *rôles*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-200">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="d16b9-201">L’exemple suivant suppose que vous avez deux bases de données sur un serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="d16b9-201">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="d16b9-202">Une base de données contient les données et le schéma de l’appartenance ASP.NET existant.</span><span class="sxs-lookup"><span data-stu-id="d16b9-202">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="d16b9-203">L’autre *CoreIdentitySample* base de données a été créée à l’aide des étapes décrites précédemment.</span><span class="sxs-lookup"><span data-stu-id="d16b9-203">The other *CoreIdentitySample* database was created using steps described earlier.</span></span> <span data-ttu-id="d16b9-204">Les commentaires sont incluses inline pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="d16b9-204">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
USE aspnetdb

-- INSERT USERS
INSERT INTO CoreIdentitySample.dbo.AspNetUsers
            (Id,
             UserName,
             NormalizedUserName,
             PasswordHash,
             SecurityStamp,
             EmailConfirmed,
             PhoneNumber,
             PhoneNumberConfirmed,
             TwoFactorEnabled,
             LockoutEnd,
             LockoutEnabled,
             AccessFailedCount,
             Email,
             NormalizedEmail)
SELECT aspnet_Users.UserId,
       aspnet_Users.UserName,
       -- The NormalizedUserName value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Users.UserName),
       -- Creates an empty password since passwords don't map between the 2 schemas
       '',
       /*
        The SecurityStamp token is used to verify the state of an account and
        is subject to change at any time. It should be initialized as a new ID.
       */
       NewID(),
       /*
        EmailConfirmed is set when a new user is created and confirmed via email.
        Users must have this set during migration to reset passwords.
       */
       1,
       aspnet_Users.MobileAlias,
       CASE
         WHEN aspnet_Users.MobileAlias IS NULL THEN 0
         ELSE 1
       END,
       -- 2FA likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         -- Setting lockout date to time in the future (1,000 years)
         WHEN aspnet_Membership.IsLockedOut = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_Membership.IsLockedOut,
       /*
        AccessFailedAccount is used to track failed logins. This is stored in
        Membership in multiple columns. Setting to 0 arbitrarily.
       */
       0,
       aspnet_Membership.Email,
       -- The NormalizedEmail value is upper case in ASP.NET Core Identity
       UPPER(aspnet_Membership.Email)
FROM   aspnet_Users
       LEFT OUTER JOIN aspnet_Membership
                    ON aspnet_Membership.ApplicationId =
                       aspnet_Users.ApplicationId
                       AND aspnet_Users.UserId = aspnet_Membership.UserId
       LEFT OUTER JOIN CoreIdentitySample.dbo.AspNetUsers
                    ON aspnet_Membership.UserId = AspNetUsers.Id
WHERE  AspNetUsers.Id IS NULL

-- INSERT ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetRoles(Id, Name)
SELECT RoleId, RoleName
FROM aspnet_Roles;

-- INSERT USER ROLES
INSERT INTO CoreIdentitySample.dbo.AspNetUserRoles(UserId, RoleId)
SELECT UserId, RoleId
FROM aspnet_UsersInRoles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="d16b9-205">Après l’achèvement du script précédent, l’application ASP.NET Core Identity créée précédemment est renseignée avec les utilisateurs d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="d16b9-205">After completion of the preceding script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="d16b9-206">Les utilisateurs doivent modifier leurs mots de passe avant de vous connecter.</span><span class="sxs-lookup"><span data-stu-id="d16b9-206">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="d16b9-207">Si le système d’appartenance avait des utilisateurs avec des noms d’utilisateur qui ne correspond pas à leur adresse de messagerie, les modifications sont requises pour l’application créée précédemment pour tenir compte de cela.</span><span class="sxs-lookup"><span data-stu-id="d16b9-207">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="d16b9-208">Le modèle par défaut attend `UserName` et `Email` soient les mêmes.</span><span class="sxs-lookup"><span data-stu-id="d16b9-208">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="d16b9-209">Dans les situations dans lesquelles ils sont différents, le processus de connexion doit être modifiée pour utiliser `UserName` au lieu de `Email`.</span><span class="sxs-lookup"><span data-stu-id="d16b9-209">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="d16b9-210">Dans le `PageModel` de la Page de connexion, situé dans *Pages\Account\Login.cshtml.cs*, supprimer le `[EmailAddress]` attribut à partir de la *E-mail* propriété.</span><span class="sxs-lookup"><span data-stu-id="d16b9-210">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="d16b9-211">Renommez-le *nom d’utilisateur*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-211">Rename it to *UserName*.</span></span> <span data-ttu-id="d16b9-212">Cela nécessite une modification dans la mesure `EmailAddress` est mentionné dans le *vue* et *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="d16b9-212">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="d16b9-213">Le résultat ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="d16b9-213">The result looks like the following:</span></span>

 ![Connexion fixe](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="d16b9-215">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d16b9-215">Next steps</span></span>

<span data-ttu-id="d16b9-216">Dans ce didacticiel, vous avez appris comment porter des utilisateurs de l’appartenance SQL vers ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="d16b9-216">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="d16b9-217">Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="d16b9-217">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
