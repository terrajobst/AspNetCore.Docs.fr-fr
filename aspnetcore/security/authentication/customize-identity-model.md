---
title: Personnalisation de modèle d’identité dans ASP.NET Core
author: ajcvickers
description: Cet article décrit comment personnaliser le modèle de données Entity Framework Core sous-jacent pour ASP.NET Core Identity.
ms.author: avickers
ms.date: 09/24/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 0aa7448ac37a97a4d09a04caf365f641f22f5997
ms.sourcegitcommit: a1c43150ed46aa01572399e8aede50d4668745ca
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58327299"
---
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="13790-103">Personnalisation de modèle d’identité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13790-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="13790-104">Par [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="13790-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="13790-105">ASP.NET Core Identity fournit une infrastructure pour la gestion et le stockage de comptes d’utilisateur dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13790-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="13790-106">Identité est ajoutée à votre projet lorsque **comptes d’utilisateur individuels** est sélectionné comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="13790-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="13790-107">Par défaut, identité utilise d’un Entity Framework (EF) modèle de données principal.</span><span class="sxs-lookup"><span data-stu-id="13790-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="13790-108">Cet article décrit comment personnaliser le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="13790-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="13790-109">Identité et les Migrations d’EF Core</span><span class="sxs-lookup"><span data-stu-id="13790-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="13790-110">Avant d’examiner le modèle, il est utile de comprendre le fonctionne de l’identité avec [Migrations d’EF Core](/ef/core/managing-schemas/migrations/) pour créer et mettre à jour une base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="13790-111">Au niveau supérieur, le processus est :</span><span class="sxs-lookup"><span data-stu-id="13790-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="13790-112">Définir ou mettre à jour un [modèle de données dans le code](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="13790-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="13790-113">Ajoutez une Migration pour traduire ce modèle en les modifications qui peuvent être appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="13790-114">Vérifiez que la Migration représente correctement vos intentions.</span><span class="sxs-lookup"><span data-stu-id="13790-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="13790-115">Appliquez la Migration pour mettre à jour de la base de données pour être synchronisé avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="13790-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="13790-116">Répétez les étapes 1 à 4 pour affiner le modèle et synchroniser la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="13790-117">Pour ajouter et appliquer des Migrations, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="13790-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="13790-118">Le **Console du Gestionnaire de Package** fenêtre (PMC) si vous utilisez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13790-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="13790-119">Pour plus d’informations, consultez [les outils EF Core PMC](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="13790-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="13790-120">L’interface CLI .NET Core si à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="13790-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="13790-121">Pour plus d’informations, consultez [outils de ligne de commande EF Core .NET](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="13790-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="13790-122">En cliquant sur le **appliquer les Migrations** bouton sur la page d’erreur lors de l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="13790-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="13790-123">ASP.NET Core possède un gestionnaire de page d’erreurs au moment du développement.</span><span class="sxs-lookup"><span data-stu-id="13790-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="13790-124">Le gestionnaire peut appliquer des migrations quand l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="13790-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="13790-125">Pour les applications de production, il est souvent plus approprié pour générer des scripts SQL à partir de la migration et de déployer des modifications de base de données en tant que partie d’un déploiement d’application et de la base de données contrôlé.</span><span class="sxs-lookup"><span data-stu-id="13790-125">For production apps, it's often more appropriate to generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="13790-126">Lorsqu’une nouvelle application à l’aide d’identité est créée, les étapes 1 et 2 ci-dessus ont déjà été effectuées.</span><span class="sxs-lookup"><span data-stu-id="13790-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="13790-127">Autrement dit, le modèle de données initiale existe déjà, et la migration initiale a été ajoutée au projet.</span><span class="sxs-lookup"><span data-stu-id="13790-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="13790-128">La migration initiale doit toujours être appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="13790-129">La migration initiale peut être appliquée via une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="13790-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="13790-130">Exécutez `Update-Database` dans PMC.</span><span class="sxs-lookup"><span data-stu-id="13790-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="13790-131">Exécutez `dotnet ef database update` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="13790-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="13790-132">Cliquez sur le **appliquer les Migrations** bouton sur la page d’erreur lors de l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="13790-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="13790-133">Répétez les étapes précédentes, comme les modifications sont apportées au modèle.</span><span class="sxs-lookup"><span data-stu-id="13790-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="13790-134">Le modèle d’identité</span><span class="sxs-lookup"><span data-stu-id="13790-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="13790-135">Types d'entité</span><span class="sxs-lookup"><span data-stu-id="13790-135">Entity types</span></span>

<span data-ttu-id="13790-136">Le modèle d’identité se compose des types d’entité suivants.</span><span class="sxs-lookup"><span data-stu-id="13790-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="13790-137">Type d'entité</span><span class="sxs-lookup"><span data-stu-id="13790-137">Entity type</span></span>|<span data-ttu-id="13790-138">Description</span><span class="sxs-lookup"><span data-stu-id="13790-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="13790-139">Représente l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="13790-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="13790-140">Représente un rôle.</span><span class="sxs-lookup"><span data-stu-id="13790-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="13790-141">Représente une revendication qui possède un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="13790-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="13790-142">Représente un jeton d’authentification pour un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="13790-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="13790-143">Associe un utilisateur à un compte de connexion.</span><span class="sxs-lookup"><span data-stu-id="13790-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="13790-144">Représente une revendication qui est accordée à tous les utilisateurs au sein d’un rôle.</span><span class="sxs-lookup"><span data-stu-id="13790-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="13790-145">Une entité de jointure qui associe des utilisateurs et des rôles.</span><span class="sxs-lookup"><span data-stu-id="13790-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="13790-146">Relations de type d’entité</span><span class="sxs-lookup"><span data-stu-id="13790-146">Entity type relationships</span></span>

<span data-ttu-id="13790-147">Le [types d’entité](#entity-types) sont liés entre eux comme suit :</span><span class="sxs-lookup"><span data-stu-id="13790-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="13790-148">Chaque `User` peut avoir de nombreux `UserClaims`.</span><span class="sxs-lookup"><span data-stu-id="13790-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="13790-149">Chaque `User` peut avoir de nombreux `UserLogins`.</span><span class="sxs-lookup"><span data-stu-id="13790-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="13790-150">Chaque `User` peut avoir de nombreux `UserTokens`.</span><span class="sxs-lookup"><span data-stu-id="13790-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="13790-151">Chaque `Role` peut avoir de nombreux associé `RoleClaims`.</span><span class="sxs-lookup"><span data-stu-id="13790-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="13790-152">Chaque `User` peut avoir de nombreux associés `Roles`et chaque `Role` peut être associé à plusieurs `Users`.</span><span class="sxs-lookup"><span data-stu-id="13790-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="13790-153">Il s’agit d’une relation plusieurs-à-plusieurs qui requiert une table de jointure dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="13790-154">La table de jointure est représentée par le `UserRole` entité.</span><span class="sxs-lookup"><span data-stu-id="13790-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="13790-155">Configuration de modèle par défaut</span><span class="sxs-lookup"><span data-stu-id="13790-155">Default model configuration</span></span>

<span data-ttu-id="13790-156">Identité définit un grand nombre *classes de contexte* qui hérite de <xref:Microsoft.EntityFrameworkCore.DbContext> pour configurer et utiliser le modèle.</span><span class="sxs-lookup"><span data-stu-id="13790-156">Identity defines many *context classes* that inherit from <xref:Microsoft.EntityFrameworkCore.DbContext> to configure and use the model.</span></span> <span data-ttu-id="13790-157">Cette configuration est effectuée à l’aide de la [EF Core API Code First Fluent](/ef/core/modeling/) dans le <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> méthode de la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="13790-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> method of the context class.</span></span> <span data-ttu-id="13790-158">La configuration par défaut est :</span><span class="sxs-lookup"><span data-stu-id="13790-158">The default configuration is:</span></span>

```csharp
builder.Entity<TUser>(b =>
{
    // Primary key
    b.HasKey(u => u.Id);

    // Indexes for "normalized" username and email, to allow efficient lookups
    b.HasIndex(u => u.NormalizedUserName).HasName("UserNameIndex").IsUnique();
    b.HasIndex(u => u.NormalizedEmail).HasName("EmailIndex");

    // Maps to the AspNetUsers table
    b.ToTable("AspNetUsers");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.UserName).HasMaxLength(256);
    b.Property(u => u.NormalizedUserName).HasMaxLength(256);
    b.Property(u => u.Email).HasMaxLength(256);
    b.Property(u => u.NormalizedEmail).HasMaxLength(256);

    // The relationships between User and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each User can have many UserClaims
    b.HasMany<TUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

    // Each User can have many UserLogins
    b.HasMany<TUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

    // Each User can have many UserTokens
    b.HasMany<TUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

    // Each User can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
});

builder.Entity<TUserClaim>(b =>
{
    // Primary key
    b.HasKey(uc => uc.Id);

    // Maps to the AspNetUserClaims table
    b.ToTable("AspNetUserClaims");
});

builder.Entity<TUserLogin>(b =>
{
    // Composite primary key consisting of the LoginProvider and the key to use
    // with that provider
    b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common DB restrictions
    b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
    b.Property(t => t.Name).HasMaxLength(maxKeyLength);

    // Maps to the AspNetUserTokens table
    b.ToTable("AspNetUserTokens");
});

builder.Entity<TRole>(b =>
{
    // Primary key
    b.HasKey(r => r.Id);

    // Index for "normalized" role name to allow efficient lookups
    b.HasIndex(r => r.NormalizedName).HasName("RoleNameIndex").IsUnique();

    // Maps to the AspNetRoles table
    b.ToTable("AspNetRoles");

    // A concurrency token for use with the optimistic concurrency checking
    b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

    // Limit the size of columns to use efficient database types
    b.Property(u => u.Name).HasMaxLength(256);
    b.Property(u => u.NormalizedName).HasMaxLength(256);

    // The relationships between Role and other entity types
    // Note that these relationships are configured with no navigation properties

    // Each Role can have many entries in the UserRole join table
    b.HasMany<TUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

    // Each Role can have many associated RoleClaims
    b.HasMany<TRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
});

builder.Entity<TRoleClaim>(b =>
{
    // Primary key
    b.HasKey(rc => rc.Id);

    // Maps to the AspNetRoleClaims table
    b.ToTable("AspNetRoleClaims");
});

builder.Entity<TUserRole>(b =>
{
    // Primary key
    b.HasKey(r => new { r.UserId, r.RoleId });

    // Maps to the AspNetUserRoles table
    b.ToTable("AspNetUserRoles");
});
```

### <a name="model-generic-types"></a><span data-ttu-id="13790-159">Modèle des types génériques</span><span class="sxs-lookup"><span data-stu-id="13790-159">Model generic types</span></span>

<span data-ttu-id="13790-160">Identité définit la valeur par défaut [Common Language Runtime](/dotnet/standard/glossary#clr) types (CLR) pour chacun des types d’entités répertoriées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="13790-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="13790-161">Ces types sont toutes le préfixe *identité*:</span><span class="sxs-lookup"><span data-stu-id="13790-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="13790-162">Au lieu d’utiliser directement ces types, les types peuvent être utilisés comme classes de base pour les types de l’application.</span><span class="sxs-lookup"><span data-stu-id="13790-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="13790-163">Le `DbContext` classes définies par l’identité sont génériques, telles que les différents types CLR peuvent être utilisés pour un ou plusieurs des types d’entités dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="13790-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="13790-164">Ces types génériques permettent également le `User` type de données de (PK) clé primaire à modifier.</span><span class="sxs-lookup"><span data-stu-id="13790-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="13790-165">Lorsque vous utilisez des identités avec prise en charge pour les rôles, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> classe doit être utilisée.</span><span class="sxs-lookup"><span data-stu-id="13790-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="13790-166">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13790-166">For example:</span></span>

```csharp
// Uses all the built-in Identity types
// Uses `string` as the key type
public class IdentityDbContext
    : IdentityDbContext<IdentityUser, IdentityRole, string>
{
}

// Uses the built-in Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityDbContext<TUser>
    : IdentityDbContext<TUser, IdentityRole, string>
        where TUser : IdentityUser
{
}

// Uses the built-in Identity types except with custom User and Role types
// The key type is defined by TKey
public class IdentityDbContext<TUser, TRole, TKey> : IdentityDbContext<
    TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>,
    IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<
    TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
    : IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
         where TUser : IdentityUser<TKey>
         where TRole : IdentityRole<TKey>
         where TKey : IEquatable<TKey>
         where TUserClaim : IdentityUserClaim<TKey>
         where TUserRole : IdentityUserRole<TKey>
         where TUserLogin : IdentityUserLogin<TKey>
         where TRoleClaim : IdentityRoleClaim<TKey>
         where TUserToken : IdentityUserToken<TKey>
```

<span data-ttu-id="13790-167">Il est également possible d’utiliser Identity sans rôles (uniquement des revendications), auquel cas un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> classe doit être utilisée :</span><span class="sxs-lookup"><span data-stu-id="13790-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

```csharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey> : IdentityUserContext<
    TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>,
    IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<
    TUser, TKey, TUserClaim, TUserLogin, TUserToken> : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customize-the-model"></a><span data-ttu-id="13790-168">Personnaliser le modèle</span><span class="sxs-lookup"><span data-stu-id="13790-168">Customize the model</span></span>

<span data-ttu-id="13790-169">Le point de départ pour la personnalisation du modèle consiste à dériver à partir du type de contexte approprié.</span><span class="sxs-lookup"><span data-stu-id="13790-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="13790-170">Consultez le [modéliser les types génériques](#model-generic-types) section.</span><span class="sxs-lookup"><span data-stu-id="13790-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="13790-171">Ce type de contexte est habituellement appelé `ApplicationDbContext` et est créé par les modèles ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13790-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="13790-172">Le contexte est utilisé pour configurer le modèle de deux manières :</span><span class="sxs-lookup"><span data-stu-id="13790-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="13790-173">Fourniture d’entité et les types de clés pour les paramètres de type générique.</span><span class="sxs-lookup"><span data-stu-id="13790-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="13790-174">Substitution de `OnModelCreating` pour modifier le mappage de ces types.</span><span class="sxs-lookup"><span data-stu-id="13790-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="13790-175">Lors de la substitution `OnModelCreating`, `base.OnModelCreating` doit être appelée en premier ; la configuration de substitution doit ensuite être appelée.</span><span class="sxs-lookup"><span data-stu-id="13790-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="13790-176">EF Core n’a généralement une stratégie last-celui-wins pour la configuration.</span><span class="sxs-lookup"><span data-stu-id="13790-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="13790-177">Par exemple, si le `ToTable` méthode pour un type d’entité est appelée en premier avec le nom d’une table et puis à nouveau avec un autre nom de table, le nom de table dans le deuxième appel est utilisé ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="13790-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="13790-178">Données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="13790-178">Custom user data</span></span>

<span data-ttu-id="13790-179">[Les données utilisateur personnalisées](xref:security/authentication/add-user-data) est pris en charge en héritant de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="13790-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="13790-180">Il est courant de nommer ce type `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="13790-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="13790-181">Utilisez le `ApplicationUser` type comme argument générique pour le contexte :</span><span class="sxs-lookup"><span data-stu-id="13790-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="13790-182">Il est inutile de substituer `OnModelCreating` dans la `ApplicationDbContext` classe.</span><span class="sxs-lookup"><span data-stu-id="13790-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="13790-183">EF Core mappe le `CustomTag` propriété par convention.</span><span class="sxs-lookup"><span data-stu-id="13790-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="13790-184">Toutefois, la base de données doit être mis à jour pour créer un nouveau `CustomTag` colonne.</span><span class="sxs-lookup"><span data-stu-id="13790-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="13790-185">Pour créer la colonne, ajoutez une migration, puis mettez à jour la base de données comme décrit dans [identité et les Migrations d’EF Core](#identity-and-ef-core-migrations).</span><span class="sxs-lookup"><span data-stu-id="13790-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="13790-186">Mise à jour `Startup.ConfigureServices` pour utiliser le nouveau `ApplicationUser` classe :</span><span class="sxs-lookup"><span data-stu-id="13790-186">Update `Startup.ConfigureServices` to use the new `ApplicationUser` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="13790-187">Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="13790-187">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="13790-188">Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="13790-188">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="13790-189">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="13790-189">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="13790-190">Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="13790-190">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="13790-191">Pour plus d'informations, voir :</span><span class="sxs-lookup"><span data-stu-id="13790-191">For more information, see:</span></span>

* [<span data-ttu-id="13790-192">Ajouter un modèle automatique d’identité</span><span class="sxs-lookup"><span data-stu-id="13790-192">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="13790-193">Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité</span><span class="sxs-lookup"><span data-stu-id="13790-193">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
        .AddDefaultTokenProviders();
```

::: moniker-end

### <a name="change-the-primary-key-type"></a><span data-ttu-id="13790-194">Modifier le type de clé primaire</span><span class="sxs-lookup"><span data-stu-id="13790-194">Change the primary key type</span></span>

<span data-ttu-id="13790-195">Une modification apportée au type de données de la colonne de clé primaire une fois que la base de données a été créée est problématique sur de nombreux systèmes de base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-195">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="13790-196">En général, la modification de la clé primaire implique de supprimer puis recréer la table.</span><span class="sxs-lookup"><span data-stu-id="13790-196">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="13790-197">Par conséquent, les types de clés doivent être spécifiés dans la migration initiale lors de la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-197">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="13790-198">Suivez ces étapes pour modifier le type de clé primaire :</span><span class="sxs-lookup"><span data-stu-id="13790-198">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="13790-199">Si la base de données a été créé avant la modification de la clé primaire, exécutez `Drop-Database` (PMC) ou `dotnet ef database drop` (CLI) .NET Core pour le supprimer.</span><span class="sxs-lookup"><span data-stu-id="13790-199">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="13790-200">Après avoir confirmé la suppression de la base de données, supprimez la migration initiale avec `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (CLI .NET Core).</span><span class="sxs-lookup"><span data-stu-id="13790-200">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="13790-201">Mise à jour le `ApplicationDbContext` classe à dériver de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span><span class="sxs-lookup"><span data-stu-id="13790-201">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="13790-202">Spécifiez le type de clé pour `TKey`.</span><span class="sxs-lookup"><span data-stu-id="13790-202">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="13790-203">Par exemple, pour utiliser un `Guid` type de clé :</span><span class="sxs-lookup"><span data-stu-id="13790-203">For example, to use a `Guid` key type:</span></span>

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    ::: moniker range=">= aspnetcore-2.0"

    <span data-ttu-id="13790-204">Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> doit être spécifié pour utiliser le nouveau type de clé.</span><span class="sxs-lookup"><span data-stu-id="13790-204">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="13790-205">Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> doit être spécifié pour utiliser le nouveau type de clé.</span><span class="sxs-lookup"><span data-stu-id="13790-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="13790-206">`Startup.ConfigureServices` doit être mis à jour pour utiliser l’utilisateur générique :</span><span class="sxs-lookup"><span data-stu-id="13790-206">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<IdentityUser<Guid>>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<IdentityUser<Guid>, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    ::: moniker-end

4. <span data-ttu-id="13790-207">Si un personnalisé `ApplicationUser` classe est utilisée, mettez à jour de la classe d’hériter de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="13790-207">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="13790-208">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13790-208">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="13790-209">Mise à jour `ApplicationDbContext` pour référencer personnalisé `ApplicationUser` classe :</span><span class="sxs-lookup"><span data-stu-id="13790-209">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

    ```csharp
    public class ApplicationDbContext
        : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
    ```

    <span data-ttu-id="13790-210">Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13790-210">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="13790-211">Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="13790-211">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="13790-212">Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="13790-212">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="13790-213">Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="13790-213">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="13790-214">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="13790-214">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="13790-215">Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="13790-215">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="13790-216">Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="13790-216">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="13790-217">Le <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> méthode accepte un `TKey` type indiquant le type de données de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="13790-217">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="13790-218">Si un personnalisé `ApplicationRole` classe est utilisée, mettez à jour de la classe d’hériter de `IdentityRole<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="13790-218">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="13790-219">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13790-219">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="13790-220">Mise à jour `ApplicationDbContext` pour référencer personnalisé `ApplicationRole` classe.</span><span class="sxs-lookup"><span data-stu-id="13790-220">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="13790-221">Par exemple, la classe suivante fait référence à un personnalisé `ApplicationUser` et personnalisé `ApplicationRole`:</span><span class="sxs-lookup"><span data-stu-id="13790-221">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="13790-222">Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13790-222">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="13790-223">Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="13790-223">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    <span data-ttu-id="13790-224">Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="13790-224">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="13790-225">Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="13790-225">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="13790-226">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="13790-226">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="13790-227">Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="13790-227">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="13790-228">Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13790-228">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="13790-229">Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.</span><span class="sxs-lookup"><span data-stu-id="13790-229">The primary key's data type is inferred by analyzing the <xref:Microsoft.EntityFrameworkCore.DbContext> object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="13790-230">Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13790-230">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="13790-231">Le <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> méthode accepte un `TKey` type indiquant le type de données de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="13790-231">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="13790-232">Ajouter des propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="13790-232">Add navigation properties</span></span>

<span data-ttu-id="13790-233">Modification de la configuration de modèle pour les relations permettre être plus difficile que d’autres modifications apportées.</span><span class="sxs-lookup"><span data-stu-id="13790-233">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="13790-234">Veillez à remplacer les relations existantes plutôt que de créer de nouvelles relations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="13790-234">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="13790-235">En particulier, la relation modifiée devez spécifier la même propriété (FK) clé étrangère en tant que la relation existante.</span><span class="sxs-lookup"><span data-stu-id="13790-235">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="13790-236">Par exemple, la relation entre `Users` et `UserClaims` est, par défaut, spécifiée comme suit :</span><span class="sxs-lookup"><span data-stu-id="13790-236">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

```csharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="13790-237">La clé étrangère pour cette relation est spécifiée comme le `UserClaim.UserId` propriété.</span><span class="sxs-lookup"><span data-stu-id="13790-237">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="13790-238">`HasMany` et `WithOne` sont appelés sans arguments pour créer la relation sans propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="13790-238">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="13790-239">Ajouter une propriété de navigation `ApplicationUser` qui permet d’associés `UserClaims` pour être référencé à partir de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="13790-239">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="13790-240">Le `TKey` pour `IdentityUserClaim<TKey>` est du type spécifié pour la clé primaire d’utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="13790-240">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="13790-241">Dans ce cas, `TKey` est `string` car les valeurs par défaut sont utilisés.</span><span class="sxs-lookup"><span data-stu-id="13790-241">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="13790-242">Il a **pas** le type de clé primaire pour la `UserClaim` type d’entité.</span><span class="sxs-lookup"><span data-stu-id="13790-242">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="13790-243">Maintenant que la propriété de navigation existe, il doit être configuré dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="13790-243">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();
        });
    }
}
```

<span data-ttu-id="13790-244">Notez que la relation est configurée exactement telle qu’elle était avant, uniquement avec une propriété de navigation spécifiée dans l’appel à `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="13790-244">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="13790-245">Les propriétés de navigation existent uniquement dans le modèle EF, pas la base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-245">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="13790-246">Étant donné que la clé étrangère pour la relation n’a pas changé, ce type de modification de modèle ne nécessite pas la base de données à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="13790-246">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="13790-247">Cela peut être vérifié en ajoutant une migration après avoir effectué la modification.</span><span class="sxs-lookup"><span data-stu-id="13790-247">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="13790-248">Le `Up` et `Down` méthodes sont vides.</span><span class="sxs-lookup"><span data-stu-id="13790-248">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="13790-249">Ajouter toutes les propriétés de navigation utilisateur</span><span class="sxs-lookup"><span data-stu-id="13790-249">Add all User navigation properties</span></span>

<span data-ttu-id="13790-250">À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation unidirectionnelle pour toutes les relations utilisateur :</span><span class="sxs-lookup"><span data-stu-id="13790-250">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne()
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });
    }
}
```

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="13790-251">Ajouter des propriétés de navigation utilisateur et de rôle</span><span class="sxs-lookup"><span data-stu-id="13790-251">Add User and Role navigation properties</span></span>

<span data-ttu-id="13790-252">À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur l’utilisateur et le rôle :</span><span class="sxs-lookup"><span data-stu-id="13790-252">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        IdentityUserClaim<string>, ApplicationUserRole, IdentityUserLogin<string>,
        IdentityRoleClaim<string>, IdentityUserToken<string>>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne()
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne()
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne()
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();
        });

    }
}
```

<span data-ttu-id="13790-253">Remarques :</span><span class="sxs-lookup"><span data-stu-id="13790-253">Notes:</span></span>

* <span data-ttu-id="13790-254">Cet exemple inclut également le `UserRole` joindre l’entité, qui est nécessaire pour naviguer jusqu'à la relation plusieurs-à-plusieurs à partir des utilisateurs aux rôles.</span><span class="sxs-lookup"><span data-stu-id="13790-254">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="13790-255">N’oubliez pas de modifier les types des propriétés de navigation pour les faire correspondre `ApplicationXxx` types sont maintenant utilisés au lieu de `IdentityXxx` types.</span><span class="sxs-lookup"><span data-stu-id="13790-255">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="13790-256">N’oubliez pas d’utiliser le `ApplicationXxx` dans le modèle générique `ApplicationContext` définition.</span><span class="sxs-lookup"><span data-stu-id="13790-256">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="13790-257">Ajouter toutes les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="13790-257">Add all navigation properties</span></span>

<span data-ttu-id="13790-258">À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur tous les types d’entités :</span><span class="sxs-lookup"><span data-stu-id="13790-258">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<ApplicationUserClaim> Claims { get; set; }
    public virtual ICollection<ApplicationUserLogin> Logins { get; set; }
    public virtual ICollection<ApplicationUserToken> Tokens { get; set; }
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
}

public class ApplicationRole : IdentityRole
{
    public virtual ICollection<ApplicationUserRole> UserRoles { get; set; }
    public virtual ICollection<ApplicationRoleClaim> RoleClaims { get; set; }
}

public class ApplicationUserRole : IdentityUserRole<string>
{
    public virtual ApplicationUser User { get; set; }
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserClaim : IdentityUserClaim<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationUserLogin : IdentityUserLogin<string>
{
    public virtual ApplicationUser User { get; set; }
}

public class ApplicationRoleClaim : IdentityRoleClaim<string>
{
    public virtual ApplicationRole Role { get; set; }
}

public class ApplicationUserToken : IdentityUserToken<string>
{
    public virtual ApplicationUser User { get; set; }
}
```

```csharp
public class ApplicationDbContext
    : IdentityDbContext<
        ApplicationUser, ApplicationRole, string,
        ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin,
        ApplicationRoleClaim, ApplicationUserToken>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<ApplicationUser>(b =>
        {
            // Each User can have many UserClaims
            b.HasMany(e => e.Claims)
                .WithOne(e => e.User)
                .HasForeignKey(uc => uc.UserId)
                .IsRequired();

            // Each User can have many UserLogins
            b.HasMany(e => e.Logins)
                .WithOne(e => e.User)
                .HasForeignKey(ul => ul.UserId)
                .IsRequired();

            // Each User can have many UserTokens
            b.HasMany(e => e.Tokens)
                .WithOne(e => e.User)
                .HasForeignKey(ut => ut.UserId)
                .IsRequired();

            // Each User can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.User)
                .HasForeignKey(ur => ur.UserId)
                .IsRequired();
        });

        modelBuilder.Entity<ApplicationRole>(b =>
        {
            // Each Role can have many entries in the UserRole join table
            b.HasMany(e => e.UserRoles)
                .WithOne(e => e.Role)
                .HasForeignKey(ur => ur.RoleId)
                .IsRequired();

            // Each Role can have many associated RoleClaims
            b.HasMany(e => e.RoleClaims)
                .WithOne(e => e.Role)
                .HasForeignKey(rc => rc.RoleId)
                .IsRequired();
        });
    }
}
```

### <a name="use-composite-keys"></a><span data-ttu-id="13790-259">Utiliser des clés composites</span><span class="sxs-lookup"><span data-stu-id="13790-259">Use composite keys</span></span>

<span data-ttu-id="13790-260">Les sections précédentes a montré la modification du type de clé utilisée dans le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="13790-260">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="13790-261">Modification du modèle de clé d’identité pour utiliser des clés composites n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="13790-261">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="13790-262">À l’aide d’une clé composite avec l’identité implique de modifier la façon dont le code de gestionnaire d’identité interagit avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="13790-262">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="13790-263">Cette personnalisation est abordée dans ce document.</span><span class="sxs-lookup"><span data-stu-id="13790-263">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="13790-264">Changer les noms de table/colonne et les facettes</span><span class="sxs-lookup"><span data-stu-id="13790-264">Change table/column names and facets</span></span>

<span data-ttu-id="13790-265">Pour modifier les noms des tables et colonnes, appelez `base.OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="13790-265">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="13790-266">Ensuite, ajoutez la configuration pour remplacer les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="13790-266">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="13790-267">Par exemple, pour modifier le nom de toutes les tables d’identité :</span><span class="sxs-lookup"><span data-stu-id="13790-267">For example, to change the name of all the Identity tables:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.ToTable("MyUsers");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.ToTable("MyUserClaims");
    });

    modelBuilder.Entity<IdentityUserLogin<string>>(b =>
    {
        b.ToTable("MyUserLogins");
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.ToTable("MyUserTokens");
    });

    modelBuilder.Entity<IdentityRole>(b =>
    {
        b.ToTable("MyRoles");
    });

    modelBuilder.Entity<IdentityRoleClaim<string>>(b =>
    {
        b.ToTable("MyRoleClaims");
    });

    modelBuilder.Entity<IdentityUserRole<string>>(b =>
    {
        b.ToTable("MyUserRoles");
    });
}
```

<span data-ttu-id="13790-268">Ces exemples utilisent les types d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="13790-268">These examples use the default Identity types.</span></span> <span data-ttu-id="13790-269">Si vous utilisez un type d’application telles que `ApplicationUser`, configurez ce type au lieu du type par défaut.</span><span class="sxs-lookup"><span data-stu-id="13790-269">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="13790-270">L’exemple suivant modifie certains noms de colonne :</span><span class="sxs-lookup"><span data-stu-id="13790-270">The following example changes some column names:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(e => e.Email).HasColumnName("EMail");
    });

    modelBuilder.Entity<IdentityUserClaim<string>>(b =>
    {
        b.Property(e => e.ClaimType).HasColumnName("CType");
        b.Property(e => e.ClaimValue).HasColumnName("CValue");
    });
}
```

<span data-ttu-id="13790-271">Certains types de colonnes de la base de données peuvent être configurés avec certains *facettes* (par exemple, la valeur maximale `string` longueur autorisée).</span><span class="sxs-lookup"><span data-stu-id="13790-271">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="13790-272">L’exemple suivant définit les longueurs maximales de colonne pour plusieurs `string` propriétés dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="13790-272">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.Entity<IdentityUser>(b =>
    {
        b.Property(u => u.UserName).HasMaxLength(128);
        b.Property(u => u.NormalizedUserName).HasMaxLength(128);
        b.Property(u => u.Email).HasMaxLength(128);
        b.Property(u => u.NormalizedEmail).HasMaxLength(128);
    });

    modelBuilder.Entity<IdentityUserToken<string>>(b =>
    {
        b.Property(t => t.LoginProvider).HasMaxLength(128);
        b.Property(t => t.Name).HasMaxLength(128);
    });
}
```

### <a name="map-to-a-different-schema"></a><span data-ttu-id="13790-273">Mapper à un schéma différent</span><span class="sxs-lookup"><span data-stu-id="13790-273">Map to a different schema</span></span>

<span data-ttu-id="13790-274">Les schémas peuvent se comporter différemment sur les fournisseurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="13790-274">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="13790-275">Pour SQL Server, la valeur par défaut consiste à créer de toutes les tables dans le *dbo* schéma.</span><span class="sxs-lookup"><span data-stu-id="13790-275">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="13790-276">Les tables peuvent être créés dans un schéma différent.</span><span class="sxs-lookup"><span data-stu-id="13790-276">The tables can be created in a different schema.</span></span> <span data-ttu-id="13790-277">Exemple :</span><span class="sxs-lookup"><span data-stu-id="13790-277">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="13790-278">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="13790-278">Lazy loading</span></span>

<span data-ttu-id="13790-279">Dans cette section, la prise en charge pour les proxys de chargement différé dans le modèle d’identité est ajoutée.</span><span class="sxs-lookup"><span data-stu-id="13790-279">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="13790-280">Le chargement différé est utile, car elle permet les propriétés de navigation à être utilisée sans la première s’assurer qu’ils sont chargés.</span><span class="sxs-lookup"><span data-stu-id="13790-280">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="13790-281">Types d’entité peuvent devenir adaptées à chargement différé de plusieurs façons, comme décrit dans la [documentation EF Core](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="13790-281">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="13790-282">Par souci de simplicité, utilisez des proxys de chargement différé, ce qui nécessite :</span><span class="sxs-lookup"><span data-stu-id="13790-282">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="13790-283">Installation de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span><span class="sxs-lookup"><span data-stu-id="13790-283">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="13790-284">Un appel à <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> dans <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span><span class="sxs-lookup"><span data-stu-id="13790-284">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.</span></span>
* <span data-ttu-id="13790-285">Types d’entité publique avec `public virtual` propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="13790-285">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="13790-286">L’exemple suivant illustre l’appel `UseLazyLoadingProxies` dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="13790-286">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="13790-287">Consultez les exemples précédents pour obtenir des conseils sur l’ajout de propriétés de navigation aux types d’entité.</span><span class="sxs-lookup"><span data-stu-id="13790-287">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13790-288">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="13790-288">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
