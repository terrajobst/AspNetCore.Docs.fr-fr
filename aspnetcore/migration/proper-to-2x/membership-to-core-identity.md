---
title: Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0
author: isaac2004
description: Découvrez comment migrer des applications ASP.NET existantes à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 82158ec500151a0bb61fb1da55a53684367d9a4e
ms.sourcegitcommit: 2e054638b69f2b14f6d67d9fa3664999172ee1b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41825705"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a><span data-ttu-id="07472-103">Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0</span><span class="sxs-lookup"><span data-stu-id="07472-103">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>

<span data-ttu-id="07472-104">De [Isaac Levin](https://isaaclevin.com)</span><span class="sxs-lookup"><span data-stu-id="07472-104">By [Isaac Levin](https://isaaclevin.com)</span></span>

<span data-ttu-id="07472-105">Cet article illustre la migration du schéma de base de données pour les applications ASP.NET à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-105">This article demonstrates migrating the database schema for ASP.NET apps using Membership authentication to ASP.NET Core 2.0 Identity.</span></span>

> [!NOTE]
> <span data-ttu-id="07472-106">Ce document fournit les étapes nécessaires pour migrer le schéma de base de données pour les applications basées sur l’appartenance ASP.NET vers le schéma de base de données utilisé pour ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-106">This document provides the steps needed to migrate the database schema for ASP.NET Membership-based apps to the database schema used for ASP.NET Core Identity.</span></span> <span data-ttu-id="07472-107">Pour plus d’informations sur la migration à partir de l’authentification basée sur l’appartenance ASP.NET vers ASP.NET Identity, consultez [migrer une application existante à partir de l’appartenance SQL vers ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span><span class="sxs-lookup"><span data-stu-id="07472-107">For more information about migrating from ASP.NET Membership-based authentication to ASP.NET Identity, see [Migrate an existing app from SQL Membership to ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity).</span></span> <span data-ttu-id="07472-108">Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity dans ASP.NET Core](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="07472-108">For more information about ASP.NET Core Identity, see [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity).</span></span>

## <a name="review-of-membership-schema"></a><span data-ttu-id="07472-109">Révision de schéma d’appartenance</span><span class="sxs-lookup"><span data-stu-id="07472-109">Review of Membership schema</span></span>

<span data-ttu-id="07472-110">Avant ASP.NET 2.0, les développeurs ont été chargés de créer l’ensemble du processus d’authentification et d’autorisation pour leurs applications.</span><span class="sxs-lookup"><span data-stu-id="07472-110">Prior to ASP.NET 2.0, developers were tasked with creating the entire authentication and authorization process for their apps.</span></span> <span data-ttu-id="07472-111">Avec ASP.NET 2.0, l’appartenance a été introduite, fournissant une solution réutilisable pour la gestion de la sécurité dans les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07472-111">With ASP.NET 2.0, Membership was introduced, providing a boilerplate solution to handling security within ASP.NET apps.</span></span> <span data-ttu-id="07472-112">Les développeurs ont désormais en mesure d’amorçage d’un schéma dans une base de données SQL Server avec le [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) commande.</span><span class="sxs-lookup"><span data-stu-id="07472-112">Developers were now able to bootstrap a schema into a SQL Server database with the [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) command.</span></span> <span data-ttu-id="07472-113">Après avoir exécuté cette commande, les tables suivantes ont été créées dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="07472-113">After running this command, the following tables were created in the database.</span></span>

  ![Tables d’appartenances](identity/_static/membership-tables.png)

<span data-ttu-id="07472-115">Pour migrer des applications existantes vers ASP.NET Core 2.0 Identity, les données dans ces tables doivent être migrés vers les tables utilisées par le nouveau schéma d’identité.</span><span class="sxs-lookup"><span data-stu-id="07472-115">To migrate existing apps to ASP.NET Core 2.0 Identity, the data in these tables needs to be migrated to the tables used by the new Identity schema.</span></span>

## <a name="aspnet-core-identity-20-schema"></a><span data-ttu-id="07472-116">ASP.NET Core Identity 2.0 schéma</span><span class="sxs-lookup"><span data-stu-id="07472-116">ASP.NET Core Identity 2.0 schema</span></span>

<span data-ttu-id="07472-117">ASP.NET Core 2.0 suit le [identité](/aspnet/identity/index) principe introduit dans ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="07472-117">ASP.NET Core 2.0 follows the [Identity](/aspnet/identity/index) principle introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="07472-118">Bien que le principe est partagé, l’implémentation entre les infrastructures est différente, même entre les versions d’ASP.NET Core (consultez [migrer d’authentification et identité vers ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span><span class="sxs-lookup"><span data-stu-id="07472-118">Though the principle is shared, the implementation between the frameworks is different, even between versions of ASP.NET Core (see [Migrate authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).</span></span>

<span data-ttu-id="07472-119">Le moyen le plus rapide pour afficher le schéma pour ASP.NET Core 2.0 Identity consiste à créer une application ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="07472-119">The fastest way to view the schema for ASP.NET Core 2.0 Identity is to create a new ASP.NET Core 2.0 app.</span></span> <span data-ttu-id="07472-120">Suivez ces étapes dans Visual Studio 2017 :</span><span class="sxs-lookup"><span data-stu-id="07472-120">Follow these steps in Visual Studio 2017:</span></span>

* <span data-ttu-id="07472-121">Sélectionnez **Fichier** > **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="07472-121">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="07472-122">Créer un nouveau **Application Web ASP.NET Core** et nommez le projet *CoreIdentitySample*.</span><span class="sxs-lookup"><span data-stu-id="07472-122">Create a new **ASP.NET Core Web Application** and name the project *CoreIdentitySample*.</span></span>
* <span data-ttu-id="07472-123">Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Web Application**.</span><span class="sxs-lookup"><span data-stu-id="07472-123">Select **ASP.NET Core 2.0** in the dropdown and then select **Web Application**.</span></span> <span data-ttu-id="07472-124">Ce modèle génère un [Pages Razor](xref:razor-pages/index) application.</span><span class="sxs-lookup"><span data-stu-id="07472-124">This template produces a [Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="07472-125">Avant de cliquer sur **OK**, cliquez sur **modifier l’authentification**.</span><span class="sxs-lookup"><span data-stu-id="07472-125">Before clicking **OK**, click **Change Authentication**.</span></span>
* <span data-ttu-id="07472-126">Choisissez **comptes d’utilisateur individuels** pour les modèles d’identité.</span><span class="sxs-lookup"><span data-stu-id="07472-126">Choose **Individual User Accounts** for the Identity templates.</span></span> <span data-ttu-id="07472-127">Enfin, cliquez sur **OK**, puis **OK**.</span><span class="sxs-lookup"><span data-stu-id="07472-127">Finally, click **OK**, then **OK**.</span></span> <span data-ttu-id="07472-128">Visual Studio crée un projet à l’aide du modèle ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-128">Visual Studio creates a project using the ASP.NET Core Identity template.</span></span>

<span data-ttu-id="07472-129">Identité de ASP.NET Core 2.0 utilise [Entity Framework Core](/ef/core) pour interagir avec la base de données stockant les données d’authentification.</span><span class="sxs-lookup"><span data-stu-id="07472-129">ASP.NET Core 2.0 Identity uses [Entity Framework Core](/ef/core) to interact with the database storing the authentication data.</span></span> <span data-ttu-id="07472-130">Afin que l’application nouvellement créée fonctionne, il doit être une base de données pour stocker ces données.</span><span class="sxs-lookup"><span data-stu-id="07472-130">In order for the newly created app to work, there needs to be a database to store this data.</span></span> <span data-ttu-id="07472-131">Après avoir créé une nouvelle application, le plus rapide pour inspecter le schéma dans un environnement de base de données consiste à créer la base de données à l’aide des migrations Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="07472-131">After creating a new app, the fastest way to inspect the schema in a database environment is to create the database using Entity Framework migrations.</span></span> <span data-ttu-id="07472-132">Ce processus crée une base de données, en local ou un autre emplacement, qui reproduit ce schéma.</span><span class="sxs-lookup"><span data-stu-id="07472-132">This process creates a database, either locally or elsewhere, which mimics that schema.</span></span> <span data-ttu-id="07472-133">Passez en revue la documentation précédente pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="07472-133">Review the preceding documentation for more information.</span></span>

<span data-ttu-id="07472-134">Pour créer une base de données avec le schéma d’ASP.NET Core Identity, exécutez le `Update-Database` commande de Visual Studio **Console du Gestionnaire de Package** fenêtre&mdash;il se trouve à **outils**  >  **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="07472-134">To create a database with the ASP.NET Core Identity schema, run the `Update-Database` command in Visual Studio's **Package Manager Console** (PMC) window&mdash;it's located at **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="07472-135">PMC prend en charge les commandes Entity Framework en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="07472-135">PMC supports running Entity Framework commands.</span></span>

<span data-ttu-id="07472-136">Les commandes Entity Framework utilisent la chaîne de connexion pour la base de données spécifiée dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="07472-136">Entity Framework commands use the connection string for the database specified in *appsettings.json*.</span></span> <span data-ttu-id="07472-137">La chaîne de connexion cible d’une base de données sur *localhost* nommé *asp-net-core-identity*.</span><span class="sxs-lookup"><span data-stu-id="07472-137">The following connection string targets a database on *localhost* named *asp-net-core-identity*.</span></span> <span data-ttu-id="07472-138">Dans ce paramètre, Entity Framework est configuré pour utiliser le `DefaultConnection` chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="07472-138">In this setting, Entity Framework is configured to use the `DefaultConnection` connection string.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

<span data-ttu-id="07472-139">Cette commande génère la base de données spécifié avec le schéma et toutes les données requises pour l’initialisation d’application.</span><span class="sxs-lookup"><span data-stu-id="07472-139">This command builds the database specified with the schema and any data needed for app initialization.</span></span> <span data-ttu-id="07472-140">L’image suivante illustre la structure de table est créée avec les étapes précédentes.</span><span class="sxs-lookup"><span data-stu-id="07472-140">The following image depicts the table structure that's created with the preceding steps.</span></span>

   ![Tables d’identité](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a><span data-ttu-id="07472-142">Migrer le schéma</span><span class="sxs-lookup"><span data-stu-id="07472-142">Migrate the schema</span></span>

<span data-ttu-id="07472-143">Il existe des différences subtiles dans les structures de table et les champs pour l’appartenance et ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-143">There are subtle differences in the table structures and fields for both Membership and ASP.NET Core Identity.</span></span> <span data-ttu-id="07472-144">Le modèle a des modifications substantielles pour l’authentification/autorisation avec les applications ASP.NET et ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07472-144">The pattern has changed substantially for authentication/authorization with ASP.NET and ASP.NET Core apps.</span></span> <span data-ttu-id="07472-145">Les objets clés sont toujours utilisés avec l’identité sont *utilisateurs* et *rôles*.</span><span class="sxs-lookup"><span data-stu-id="07472-145">The key objects that are still used with Identity are *Users* and *Roles*.</span></span> <span data-ttu-id="07472-146">Voici les tables de mappage pour *utilisateurs*, *rôles*, et *UserRoles*.</span><span class="sxs-lookup"><span data-stu-id="07472-146">Here are mapping tables for *Users*, *Roles*, and *UserRoles*.</span></span>

### <a name="users"></a><span data-ttu-id="07472-147">Utilisateurs</span><span class="sxs-lookup"><span data-stu-id="07472-147">Users</span></span>

| <span data-ttu-id="07472-148">*Identity(AspNetUsers)*</span><span class="sxs-lookup"><span data-stu-id="07472-148">*Identity(AspNetUsers)*</span></span> |   | <span data-ttu-id="07472-149">*Membership(aspnet_Users/aspnet_Membership)*</span><span class="sxs-lookup"><span data-stu-id="07472-149">*Membership(aspnet_Users/aspnet_Membership)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="07472-150">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-150">**Field Name**</span></span> | <span data-ttu-id="07472-151">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-151">**Type**</span></span>  |   <span data-ttu-id="07472-152">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-152">**Field Name**</span></span> | <span data-ttu-id="07472-153">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-153">**Type**</span></span>  |
|`Id` | <span data-ttu-id="07472-154">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-154">string</span></span> | `aspnet_Users.UserId` | <span data-ttu-id="07472-155">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-155">string</span></span>
|`UserName` | <span data-ttu-id="07472-156">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-156">string</span></span> | `aspnet_Users.UserName` | <span data-ttu-id="07472-157">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-157">string</span></span>
|`Email` | <span data-ttu-id="07472-158">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-158">string</span></span> | `aspnet_Membership.Email` | <span data-ttu-id="07472-159">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-159">string</span></span>
|`NormalizedUserName` | <span data-ttu-id="07472-160">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-160">string</span></span> | `aspnet_Users.LoweredUserName` | <span data-ttu-id="07472-161">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-161">string</span></span>
|`NormalizedEmail` | <span data-ttu-id="07472-162">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-162">string</span></span> | `aspnet_Membership.LoweredEmail` | <span data-ttu-id="07472-163">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-163">string</span></span>
|`PhoneNumber` | <span data-ttu-id="07472-164">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-164">string</span></span> | `aspnet_Users.MobileAlias` | <span data-ttu-id="07472-165">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-165">string</span></span>
|`LockoutEnabled` | <span data-ttu-id="07472-166">bit</span><span class="sxs-lookup"><span data-stu-id="07472-166">bit</span></span> | `aspnet_Membership.IsLockedOut` | <span data-ttu-id="07472-167">bit</span><span class="sxs-lookup"><span data-stu-id="07472-167">bit</span></span>

> [!NOTE]
> <span data-ttu-id="07472-168">Pas tous les mappages de champs ressemblent à des relations un à un à partir de l’appartenance à ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-168">Not all the field mappings resemble one-to-one relationships from Membership to ASP.NET Core Identity.</span></span> <span data-ttu-id="07472-169">Le tableau précédent prend le schéma utilisateur d’appartenance par défaut et le mappe au schéma ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-169">The preceding table takes the default Membership User schema and maps it to the ASP.NET Core Identity schema.</span></span> <span data-ttu-id="07472-170">Tous les autres champs personnalisés qui ont été utilisés pour l’appartenance doivent être mappées manuellement.</span><span class="sxs-lookup"><span data-stu-id="07472-170">Any other custom fields that were used for Membership need to be mapped manually.</span></span> <span data-ttu-id="07472-171">Dans ce mappage, il n’existe aucun mappage pour les mots de passe, comme critères de mot de passe et le mot de passe sels ne migrent entre les deux.</span><span class="sxs-lookup"><span data-stu-id="07472-171">In this mapping, there's no map for passwords, as both password criteria and password salts don't migrate between the two.</span></span> <span data-ttu-id="07472-172">**Il est recommandé de laisser le mot de passe avec la valeur null et demander aux utilisateurs de réinitialiser leurs mots de passe.**</span><span class="sxs-lookup"><span data-stu-id="07472-172">**It's recommended to leave the password as null and to ask users to reset their passwords.**</span></span> <span data-ttu-id="07472-173">Dans ASP.NET Core Identity, `LockoutEnd` doit être définie par une date à l’avenir, si l’utilisateur est verrouillé. Ceci est illustré dans le script de migration.</span><span class="sxs-lookup"><span data-stu-id="07472-173">In ASP.NET Core Identity, `LockoutEnd` should be set to some date in the future if the user is locked out. This is shown in the migration script.</span></span>

### <a name="roles"></a><span data-ttu-id="07472-174">Rôles</span><span class="sxs-lookup"><span data-stu-id="07472-174">Roles</span></span>

| <span data-ttu-id="07472-175">*Identity(AspNetRoles)*</span><span class="sxs-lookup"><span data-stu-id="07472-175">*Identity(AspNetRoles)*</span></span> |   | <span data-ttu-id="07472-176">*Membership(aspnet_Roles)*</span><span class="sxs-lookup"><span data-stu-id="07472-176">*Membership(aspnet_Roles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="07472-177">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-177">**Field Name**</span></span> | <span data-ttu-id="07472-178">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-178">**Type**</span></span>  |   <span data-ttu-id="07472-179">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-179">**Field Name**</span></span> | <span data-ttu-id="07472-180">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-180">**Type**</span></span>  |
|`Id` | <span data-ttu-id="07472-181">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-181">string</span></span> | `RoleId` | <span data-ttu-id="07472-182">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-182">string</span></span>
|`Name` | <span data-ttu-id="07472-183">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-183">string</span></span> | `RoleName` | <span data-ttu-id="07472-184">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-184">string</span></span>
|`NormalizedName` | <span data-ttu-id="07472-185">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-185">string</span></span> | `LoweredRoleName` | <span data-ttu-id="07472-186">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-186">string</span></span>

### <a name="user-roles"></a><span data-ttu-id="07472-187">Rôles d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="07472-187">User Roles</span></span>

| <span data-ttu-id="07472-188">*Identity(AspNetUserRoles)*</span><span class="sxs-lookup"><span data-stu-id="07472-188">*Identity(AspNetUserRoles)*</span></span> |   | <span data-ttu-id="07472-189">*Membership(aspnet_UsersInRoles)*</span><span class="sxs-lookup"><span data-stu-id="07472-189">*Membership(aspnet_UsersInRoles)*</span></span> ||
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="07472-190">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-190">**Field Name**</span></span> | <span data-ttu-id="07472-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-191">**Type**</span></span>  |   <span data-ttu-id="07472-192">**Nom de champ**</span><span class="sxs-lookup"><span data-stu-id="07472-192">**Field Name**</span></span> | <span data-ttu-id="07472-193">**Type**</span><span class="sxs-lookup"><span data-stu-id="07472-193">**Type**</span></span>  |
|`RoleId` | <span data-ttu-id="07472-194">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-194">string</span></span> | `RoleId` | <span data-ttu-id="07472-195">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-195">string</span></span>
|`UserId` | <span data-ttu-id="07472-196">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-196">string</span></span> | `UserId` | <span data-ttu-id="07472-197">chaîne</span><span class="sxs-lookup"><span data-stu-id="07472-197">string</span></span>

<span data-ttu-id="07472-198">Référencez les tables de mappage précédent lors de la création d’un script de migration pour *utilisateurs* et *rôles*.</span><span class="sxs-lookup"><span data-stu-id="07472-198">Reference the preceding mapping tables when creating a migration script for *Users* and *Roles*.</span></span> <span data-ttu-id="07472-199">L’exemple suivant suppose que vous avez deux bases de données sur un serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="07472-199">The following example assumes you have two databases on a database server.</span></span> <span data-ttu-id="07472-200">Une base de données contient les données et le schéma de l’appartenance ASP.NET existant.</span><span class="sxs-lookup"><span data-stu-id="07472-200">One database contains the existing ASP.NET Membership schema and data.</span></span> <span data-ttu-id="07472-201">La base de données a été créé à l’aide des étapes décrites précédemment.</span><span class="sxs-lookup"><span data-stu-id="07472-201">The other database was created using steps described earlier.</span></span> <span data-ttu-id="07472-202">Les commentaires sont incluses inline pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="07472-202">Comments are included inline for more details.</span></span>

```sql
-- THIS SCRIPT NEEDS TO RUN FROM THE CONTEXT OF THE MEMBERSHIP DB
BEGIN TRANSACTION MigrateUsersAndRoles
use aspnetdb

-- INSERT USERS
INSERT INTO coreidentity.dbo.aspnetusers
            (id,
             username,
             normalizedusername,
             passwordhash,
             securitystamp,
             emailconfirmed,
             phonenumber,
             phonenumberconfirmed,
             twofactorenabled,
             lockoutend,
             lockoutenabled,
             accessfailedcount,
             email,
             normalizedemail)
SELECT aspnet_users.userid,
       aspnet_users.username,
       aspnet_users.loweredusername,
       --Creates an empty password since passwords don't map between the two schemas
       '',
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be initialized as a new ID.
       NewID(),
       --EmailConfirmed is set when a new user is created and confirmed via email. Users must have this set during migration to ensure they're able to reset passwords.
       1,
       aspnet_users.mobilealias,
       CASE
         WHEN aspnet_Users.MobileAlias is null THEN 0
         ELSE 1
       END,
       --2-factor Auth likely wasn't setup in Membership for users, so setting as false.
       0,
       CASE
         --Setting lockout date to time in the future (1000 years)
         WHEN aspnet_membership.islockedout = 1 THEN Dateadd(year, 1000,
                                                     Sysutcdatetime())
         ELSE NULL
       END,
       aspnet_membership.islockedout,
       --AccessFailedAccount is used to track failed logins. This is stored in membership in multiple columns. Setting to 0 arbitrarily.
       0,
       aspnet_membership.email,
       aspnet_membership.loweredemail
FROM   aspnet_users
       LEFT OUTER JOIN aspnet_membership
                    ON aspnet_membership.applicationid =
                       aspnet_users.applicationid
                       AND aspnet_users.userid = aspnet_membership.userid
       LEFT OUTER JOIN coreidentity.dbo.aspnetusers
                    ON aspnet_membership.userid = aspnetusers.id
WHERE  aspnetusers.id IS NULL

-- INSERT ROLES
INSERT INTO coreIdentity.dbo.aspnetroles(id,name)
SELECT roleId,rolename
FROM aspnet_roles;

-- INSERT USER ROLES
INSERT INTO coreidentity.dbo.aspnetuserroles(userid,roleid)
SELECT userid,roleid
FROM aspnet_usersinroles;

IF @@ERROR <> 0
  BEGIN
    ROLLBACK TRANSACTION MigrateUsersAndRoles
    RETURN
  END

COMMIT TRANSACTION MigrateUsersAndRoles
```

<span data-ttu-id="07472-203">Après l’achèvement de ce script, l’application ASP.NET Core Identity créée précédemment est renseignée avec les utilisateurs d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="07472-203">After completion of this script, the ASP.NET Core Identity app created earlier is populated with Membership users.</span></span> <span data-ttu-id="07472-204">Les utilisateurs doivent modifier leurs mots de passe avant de vous connecter.</span><span class="sxs-lookup"><span data-stu-id="07472-204">Users need to change their passwords before logging in.</span></span>

> [!NOTE]
> <span data-ttu-id="07472-205">Si le système d’appartenance avait des utilisateurs avec des noms d’utilisateur qui ne correspond pas à leur adresse de messagerie, les modifications sont requises pour l’application créée précédemment pour tenir compte de cela.</span><span class="sxs-lookup"><span data-stu-id="07472-205">If the Membership system had users with user names that didn't match their email address, changes are required to the app created earlier to accommodate this.</span></span> <span data-ttu-id="07472-206">Le modèle par défaut attend `UserName` et `Email` soient les mêmes.</span><span class="sxs-lookup"><span data-stu-id="07472-206">The default template expects `UserName` and `Email` to be the same.</span></span> <span data-ttu-id="07472-207">Dans les situations dans lesquelles ils sont différents, le processus de connexion doit être modifiée pour utiliser `UserName` au lieu de `Email`.</span><span class="sxs-lookup"><span data-stu-id="07472-207">For situations in which they're different, the login process needs to be modified to use `UserName` instead of `Email`.</span></span>

<span data-ttu-id="07472-208">Dans le `PageModel` de la Page de connexion, situé dans *Pages\Account\Login.cshtml.cs*, supprimer le `[EmailAddress]` attribut à partir de la *E-mail* propriété.</span><span class="sxs-lookup"><span data-stu-id="07472-208">In the `PageModel` of the Login Page, located at *Pages\Account\Login.cshtml.cs*, remove the `[EmailAddress]` attribute from the *Email* property.</span></span> <span data-ttu-id="07472-209">Renommez-le *nom d’utilisateur*.</span><span class="sxs-lookup"><span data-stu-id="07472-209">Rename it to *UserName*.</span></span> <span data-ttu-id="07472-210">Cela nécessite une modification dans la mesure `EmailAddress` est mentionné dans le *vue* et *PageModel*.</span><span class="sxs-lookup"><span data-stu-id="07472-210">This requires a change wherever `EmailAddress` is mentioned, in the *View* and *PageModel*.</span></span> <span data-ttu-id="07472-211">Le résultat ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="07472-211">The result looks like the following:</span></span>

 ![Connexion fixe](identity/_static/fixed-login.png)

## <a name="next-steps"></a><span data-ttu-id="07472-213">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="07472-213">Next steps</span></span>

<span data-ttu-id="07472-214">Dans ce didacticiel, vous avez appris comment porter des utilisateurs de l’appartenance SQL vers ASP.NET Core 2.0 Identity.</span><span class="sxs-lookup"><span data-stu-id="07472-214">In this tutorial, you learned how to port users from SQL membership to ASP.NET Core 2.0 Identity.</span></span> <span data-ttu-id="07472-215">Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity](xref:security/authentication/identity).</span><span class="sxs-lookup"><span data-stu-id="07472-215">For more information regarding ASP.NET Core Identity, see [Introduction to Identity](xref:security/authentication/identity).</span></span>
