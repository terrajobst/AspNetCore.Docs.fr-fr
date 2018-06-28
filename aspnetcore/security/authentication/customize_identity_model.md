---
title: Personnalisation de modèle d’identité
author: ajcvickers
description: Cet article décrit comment personnaliser le modèle de données Entity Framework Core sous-jacent pour ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/12/2018
ms.prod: asp.net-core
uid: security/authentication/customize_identity_model
ms.openlocfilehash: b44b4fd0f24d245b969588a7226ea6aacbe2a722
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036892"
---
# <a name="identity-model-customization"></a><span data-ttu-id="4a47a-103">Personnalisation de modèle d’identité</span><span class="sxs-lookup"><span data-stu-id="4a47a-103">Identity model customization</span></span>

<span data-ttu-id="4a47a-104">Par [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="4a47a-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="4a47a-105">Identité de ASP.NET Core fournit une infrastructure pour la gestion et le stockage des comptes d’utilisateur dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a47a-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core applications.</span></span> <span data-ttu-id="4a47a-106">Identité est ajoutée à votre projet lorsque « Comptes d’utilisateur individuels » est sélectionné comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="4a47a-106">Identity is added to your project when "Individual User Accounts" is selected as the authentication mechanism.</span></span> <span data-ttu-id="4a47a-107">Par défaut, identité utilise d’un Entity Framework (EF) modèle de données principal.</span><span class="sxs-lookup"><span data-stu-id="4a47a-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="4a47a-108">Cet article décrit comment personnaliser le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="4a47a-108">This article describes how to customize the Identity model.</span></span>

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="4a47a-109">Identité et les Migrations de noyaux EF</span><span class="sxs-lookup"><span data-stu-id="4a47a-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="4a47a-110">Avant d’examiner le modèle, il est utile de comprendre le fonctionne d’identité avec [EF Core Migrations](/ef/core/managing-schemas/migrations/) pour créer et mettre à jour une base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="4a47a-111">Au niveau supérieur, le processus est :</span><span class="sxs-lookup"><span data-stu-id="4a47a-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="4a47a-112">Définir ou mettre à jour un [modèle de données dans le code](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="4a47a-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="4a47a-113">Ajoutez une migration pour ce modèle se traduire par les modifications qui peuvent être appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-113">Add a migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="4a47a-114">Vérifiez que la migration représente correctement vos intentions.</span><span class="sxs-lookup"><span data-stu-id="4a47a-114">Check that the migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="4a47a-115">Appliquer la migration pour mettre à jour la base de données pour être synchronisé avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="4a47a-115">Apply the migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="4a47a-116">Répétez les étapes 1 à 4 pour affiner le modèle et synchroniser la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-116">Repeat steps 1-4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="4a47a-117">Les migrations sont ajoutées et appliquées à l’aide de :</span><span class="sxs-lookup"><span data-stu-id="4a47a-117">Migrations are added and applied using:</span></span>

* <span data-ttu-id="4a47a-118">Le [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) si vous utilisez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a47a-118">The [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) if you are using Visual Studio.</span></span>
* <span data-ttu-id="4a47a-119">Le [dotnet commandes](/ef/core/miscellaneous/cli/dotnet) sur la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="4a47a-119">The [dotnet commands](/ef/core/miscellaneous/cli/dotnet) on the command line.</span></span>
* <span data-ttu-id="4a47a-120">En sélectionnant le lien de migrations sur la page d’erreur lors de l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-120">Selecting the migrations link on the error page when the application is run.</span></span>

<span data-ttu-id="4a47a-121">ASP.NET Core a un gestionnaire de page d’erreur de délai de développement qui peut être utilisé pour appliquer des migrations lorsque l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-121">ASP.NET Core has a development-time error page handler that can be used to apply migrations when the application is run.</span></span> <span data-ttu-id="4a47a-122">Pour les applications de production, il est souvent plus approprié pour générer des scripts SQL à partir de la migration et les déployer à la base de données dans le cadre d’un déploiement d’application et de base de données contrôlé.</span><span class="sxs-lookup"><span data-stu-id="4a47a-122">For production applications, it is often more appropriate to generate SQL scripts from the migrations and deploy these to the database as part of a controlled application and database deployment.</span></span>

<span data-ttu-id="4a47a-123">Lorsqu’une nouvelle application à l’aide d’identité est créée, les étapes 1 et 2 ci-dessus ont déjà été effectuées.</span><span class="sxs-lookup"><span data-stu-id="4a47a-123">When a new application using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="4a47a-124">Autrement dit, le modèle de données initiale existe déjà, et la migration initiale a été ajoutée au projet.</span><span class="sxs-lookup"><span data-stu-id="4a47a-124">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="4a47a-125">La migration initiale doit toujours être appliqué à la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-125">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="4a47a-126">La migration initiale peut être effectuée en exécutant la base de données de mise à jour (PMC), la commande de mise à jour de (.NET Core CLI) dotnet ef de base de données, ou à l’aide de la page d’erreur lors de l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-126">The initial migration can be done by running the Update-Database (PMC), the dotnet ef database update (.NET Core CLI) command, or by using the error page when the application is run.</span></span>

<span data-ttu-id="4a47a-127">Les étapes précédentes doivent être répété lorsque des modifications sont apportées au modèle.</span><span class="sxs-lookup"><span data-stu-id="4a47a-127">The preceding steps need to be repeated as changes are made to the model.</span></span>

<a name="identity-model"></a>

## <a name="the-identity-model"></a><span data-ttu-id="4a47a-128">Le modèle d’identité</span><span class="sxs-lookup"><span data-stu-id="4a47a-128">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="4a47a-129">Types d'entité</span><span class="sxs-lookup"><span data-stu-id="4a47a-129">Entity types</span></span>

<span data-ttu-id="4a47a-130">Le modèle d’identité se compose de sept types d’entités :</span><span class="sxs-lookup"><span data-stu-id="4a47a-130">The Identity model consists of seven entity types:</span></span>

* <span data-ttu-id="4a47a-131">`User` -représente l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="4a47a-131">`User` - represents the user</span></span>
* <span data-ttu-id="4a47a-132">`Role` -représente un rôle</span><span class="sxs-lookup"><span data-stu-id="4a47a-132">`Role` - represents a role</span></span>
* <span data-ttu-id="4a47a-133">`UserClaim` -représente une revendication possession par l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="4a47a-133">`UserClaim` - represents a claim that a user possess</span></span>
* <span data-ttu-id="4a47a-134">`UserToken` -représente un jeton d’authentification pour un utilisateur</span><span class="sxs-lookup"><span data-stu-id="4a47a-134">`UserToken` - represents an authentication token for a user</span></span>
* <span data-ttu-id="4a47a-135">`UserLogin` -associe un utilisateur avec un compte de connexion</span><span class="sxs-lookup"><span data-stu-id="4a47a-135">`UserLogin` - associates a user with a login</span></span>
* <span data-ttu-id="4a47a-136">`RoleClaim` -représente une revendication qui est accordée à tous les utilisateurs au sein d’un rôle</span><span class="sxs-lookup"><span data-stu-id="4a47a-136">`RoleClaim` - represents a claim that is granted to all users within a role</span></span>
* <span data-ttu-id="4a47a-137">`UserRole` -joindre l’entité qui associe des utilisateurs et des rôles</span><span class="sxs-lookup"><span data-stu-id="4a47a-137">`UserRole` - join entity that associates users and roles</span></span>

### <a name="entity-type-relationships"></a><span data-ttu-id="4a47a-138">Relations de type d’entité</span><span class="sxs-lookup"><span data-stu-id="4a47a-138">Entity type relationships</span></span>

<span data-ttu-id="4a47a-139">Ces types d’entités sont associées à l’autre des manières suivantes :</span><span class="sxs-lookup"><span data-stu-id="4a47a-139">These entity types are related to each other in the following ways:</span></span>

* <span data-ttu-id="4a47a-140">Chaque `User` peut avoir de nombreux `UserClaims`</span><span class="sxs-lookup"><span data-stu-id="4a47a-140">Each `User` can have many `UserClaims`</span></span>
* <span data-ttu-id="4a47a-141">Chaque `User` peut avoir de nombreux `UserLogins`</span><span class="sxs-lookup"><span data-stu-id="4a47a-141">Each `User` can have many `UserLogins`</span></span>
* <span data-ttu-id="4a47a-142">Chaque `User` peut avoir de nombreux `UserTokens`</span><span class="sxs-lookup"><span data-stu-id="4a47a-142">Each `User` can have many `UserTokens`</span></span>
* <span data-ttu-id="4a47a-143">Chaque `Role` peut avoir de nombreux associés `RoleClaims`</span><span class="sxs-lookup"><span data-stu-id="4a47a-143">Each `Role` can have many associated `RoleClaims`</span></span>
* <span data-ttu-id="4a47a-144">Chaque `User` peut avoir de nombreux associés `Roles`et chaque `Role` peut être associé à de nombreux utilisateurs</span><span class="sxs-lookup"><span data-stu-id="4a47a-144">Each `User` can have many associated `Roles`, and each `Role` can be associated with many Users</span></span>
  * <span data-ttu-id="4a47a-145">Il s’agit d’une relation plusieurs-à-plusieurs, ce qui nécessite une table de jointure dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-145">This is a many-to-many relationship, which requires a join table in the database.</span></span> <span data-ttu-id="4a47a-146">La table de jointure est représentée par la `UserRole` entité.</span><span class="sxs-lookup"><span data-stu-id="4a47a-146">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="4a47a-147">Configuration de modèle par défaut</span><span class="sxs-lookup"><span data-stu-id="4a47a-147">Default model configuration</span></span>

<span data-ttu-id="4a47a-148">Identité définit une série de « classes de contexte » qui héritent de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) pour configurer et utiliser le modèle.</span><span class="sxs-lookup"><span data-stu-id="4a47a-148">Identity defines a variety of "context classes" which inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="4a47a-149">Cette configuration est effectuée à l’aide de la [EF Core Code première API Fluent](/ef/core/modeling/) dans les [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) méthode de la classe de contexte.</span><span class="sxs-lookup"><span data-stu-id="4a47a-149">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) method of the context class.</span></span> <span data-ttu-id="4a47a-150">La configuration par défaut est la suivante :</span><span class="sxs-lookup"><span data-stu-id="4a47a-150">The default configuration is:</span></span>

```CSharp
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

    // Limit the size of the composite key columns due to common database restrictions
    b.Property(l => l.LoginProvider).HasMaxLength(128);
    b.Property(l => l.ProviderKey).HasMaxLength(128);

    // Maps to the AspNetUserLogins table
    b.ToTable("AspNetUserLogins");
});

builder.Entity<TUserToken>(b =>
{
    // Composite primary key consisting of the UserId, LoginProvider and Name
    b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

    // Limit the size of the composite key columns due to common database restrictions
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

### <a name="model-generic-types"></a><span data-ttu-id="4a47a-151">Modèle des types génériques</span><span class="sxs-lookup"><span data-stu-id="4a47a-151">Model generic types</span></span>

<span data-ttu-id="4a47a-152">Identité définit les types CLR par défaut pour chacun des types d’entités répertoriées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="4a47a-152">Identity defines default CLR types for each of the entity types listed above.</span></span> <span data-ttu-id="4a47a-153">Ces types sont toutes le préfixe « Identité » : `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, et `IdentityUserRole`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-153">These types are all prefixed with "Identity": `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, and `IdentityUserRole`.</span></span>

<span data-ttu-id="4a47a-154">Au lieu d’utiliser directement à ces types, les types peuvent être utilisés comme classes de base pour les types de l’application.</span><span class="sxs-lookup"><span data-stu-id="4a47a-154">Rather than using these types directly, the types can be used as base classes for the application's own types.</span></span> <span data-ttu-id="4a47a-155">Le `DbContext` classes définies par l’identité sont génériques, telles que les différents types CLR peuvent être utilisés pour un ou plusieurs types d’entités dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="4a47a-155">The `DbContext` classes defined by Identity are generic such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="4a47a-156">Ces types génériques permettent également le type de la clé primaire d’utilisateur à modifier.</span><span class="sxs-lookup"><span data-stu-id="4a47a-156">These generic types also allow for the type of the User primary key to be changed.</span></span>

<span data-ttu-id="4a47a-157">Lors de l’utilisation d’identité avec prise en charge pour les rôles, un [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) classe doit être utilisée :</span><span class="sxs-lookup"><span data-stu-id="4a47a-157">When using Identity with support for roles, an [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) class should be used:</span></span>

```CSharp
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
public class IdentityDbContext<TUser, TRole, TKey>
    : IdentityDbContext<TUser, TRole, TKey, IdentityUserClaim<TKey>, IdentityUserRole<TKey>, IdentityUserLogin<TKey>, IdentityRoleClaim<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TRole : IdentityRole<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments
// The key type is defined by TKey
public abstract class IdentityDbContext<TUser, TRole, TKey, TUserClaim, TUserRole, TUserLogin, TRoleClaim, TUserToken>
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

<span data-ttu-id="4a47a-158">Il est également possible d’utiliser des identités sans rôles (uniquement des revendications), auquel cas un `IdentityUserContext` classe doit être utilisée :</span><span class="sxs-lookup"><span data-stu-id="4a47a-158">It is also possible to use Identity without roles (only claims), in which case an `IdentityUserContext` class should be used:</span></span>


```CSharp
// Uses the built-in non-role Identity types except with a custom User type
// Uses `string` as the key type
public class IdentityUserContext<TUser>
    : IdentityUserContext<TUser, string>
        where TUser : IdentityUser
{
}

// Uses the built-in non-role Identity types except with a custom User type
// The key type is defined by TKey
public class IdentityUserContext<TUser, TKey>
    : IdentityUserContext<TUser, TKey, IdentityUserClaim<TKey>, IdentityUserLogin<TKey>, IdentityUserToken<TKey>>
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
{
}

// No built-in Identity types are used; all are specified by generic arguments, with no roles
// The key type is defined by TKey
public abstract class IdentityUserContext<TUser, TKey, TUserClaim, TUserLogin, TUserToken>
    : DbContext
        where TUser : IdentityUser<TKey>
        where TKey : IEquatable<TKey>
        where TUserClaim : IdentityUserClaim<TKey>
        where TUserLogin : IdentityUserLogin<TKey>
        where TUserToken : IdentityUserToken<TKey>
{
}
```

## <a name="customizing-the-model"></a><span data-ttu-id="4a47a-159">La personnalisation du modèle</span><span class="sxs-lookup"><span data-stu-id="4a47a-159">Customizing the model</span></span>

<span data-ttu-id="4a47a-160">Le point de départ pour personnaliser le modèle doit dériver du type de contexte approprié ; consultez la section précédente.</span><span class="sxs-lookup"><span data-stu-id="4a47a-160">The starting point for customizing the model is to derive from the appropriate context type; see the preceding section.</span></span> <span data-ttu-id="4a47a-161">Ce type de contexte est généralement appelé `ApplicationDbContext` et est créé par les modèles ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a47a-161">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="4a47a-162">Le contexte est utilisé pour configurer le modèle de deux façons :</span><span class="sxs-lookup"><span data-stu-id="4a47a-162">The context is used to configure the model in two ways:</span></span>
* <span data-ttu-id="4a47a-163">En fournissant des types d’entité et la clé pour les paramètres de type générique.</span><span class="sxs-lookup"><span data-stu-id="4a47a-163">By supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="4a47a-164">En substituant `OnModelCreating` pour modifier le mappage de ces types.</span><span class="sxs-lookup"><span data-stu-id="4a47a-164">By overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="4a47a-165">Lors de la substitution `OnModelCreating`, `base.OnModelCreating` doit être appelée en premier lieu, la substitution de configuration doit ensuite être appelée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-165">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first, the overriding configuration should be called next.</span></span> <span data-ttu-id="4a47a-166">EF Core a généralement une stratégie d’un dernier-wins pour la configuration.</span><span class="sxs-lookup"><span data-stu-id="4a47a-166">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="4a47a-167">Par exemple, si le `ToTable` pour une entité de type est appelé tout d’abord avec le nom d’une table et puis plus tard avec un autre nom de table, puis le nom de table dans le deuxième appel est celle qui est utilisée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-167">For example, if the `ToTable` for an entity type is called first with one table name and then again later with a different table name, then the table name in the second call is the one that is used.</span></span>

### <a name="using-a-custom-user-type"></a><span data-ttu-id="4a47a-168">À l’aide d’un type d’utilisateur personnalisé</span><span class="sxs-lookup"><span data-stu-id="4a47a-168">Using a custom User type</span></span>

<span data-ttu-id="4a47a-169">Pour utiliser un type d’utilisateur personnalisé, créez le type et qu’il hérite de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-169">To use a custom User type, create the type and have it inherit from `IdentityUser`.</span></span> <span data-ttu-id="4a47a-170">Il est habituel de ce type de nom `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-170">It is customary to name this type `ApplicationUser`.</span></span> <span data-ttu-id="4a47a-171">Ce type ont généralement des propriétés supplémentaires pas du type de base, sinon il ne serait aucune valeur dans sa création.</span><span class="sxs-lookup"><span data-stu-id="4a47a-171">This type will typically have additional properties not in the base type, otherwise there would be no value in creating it.</span></span> <span data-ttu-id="4a47a-172">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4a47a-172">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="4a47a-173">Ensuite, utilisez ce type comme argument générique pour le contexte :</span><span class="sxs-lookup"><span data-stu-id="4a47a-173">Next use this type as a generic argument for the context:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="4a47a-174">Mise à jour `ConfigureServices` pour utiliser le nouveau `ApplicationUser` classe :</span><span class="sxs-lookup"><span data-stu-id="4a47a-174">Update `ConfigureServices` to use the new `ApplicationUser` class:</span></span>

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="4a47a-175">Il n’est pas nécessaire de substituer `OnModelCreating` ici car EF Core mappera le `CustomTag` propriété par convention.</span><span class="sxs-lookup"><span data-stu-id="4a47a-175">There is no need to override `OnModelCreating` here because EF Core will map the `CustomTag` property by convention.</span></span> <span data-ttu-id="4a47a-176">Toutefois, la base de données devra être mise à jour pour obtenir un nouveau `CustomTag` colonne.</span><span class="sxs-lookup"><span data-stu-id="4a47a-176">However, the database will need to be updated to get a new `CustomTag` column.</span></span> <span data-ttu-id="4a47a-177">Pour ce faire, ajoutez une migration, puis mettez à jour la base de données comme décrit dans [identité et EF Core Migrations](#identity-migrations).</span><span class="sxs-lookup"><span data-stu-id="4a47a-177">To do this, add a migration, and then update the database as described in  [Identity and EF Core Migrations](#identity-migrations).</span></span>

### <a name="changing-the-key-type"></a><span data-ttu-id="4a47a-178">Modification du type de clé</span><span class="sxs-lookup"><span data-stu-id="4a47a-178">Changing the key type</span></span>

<span data-ttu-id="4a47a-179">Modification du type de colonne clé primaire (PK) après avoir créé la base de données pose sur de nombreux systèmes de base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-179">Changing the type of a primary key (PK) column after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="4a47a-180">La modification de la clé publique généralement implique et recréer la table.</span><span class="sxs-lookup"><span data-stu-id="4a47a-180">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="4a47a-181">Par conséquent, il est recommandé que les types de clés soient spécifiées dans la migration initiale telles que les types de clés cible sont créés lors de la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-181">Therefore, it is recommended that key types be specified in the initial migration such that the target key types are created when the database is created.</span></span>

<span data-ttu-id="4a47a-182">Si la base de données a été créé, puis `Drop-Database` (PMC) ou `dotnet ef database drop` (.NET Core CLI) peut servir à supprimer.</span><span class="sxs-lookup"><span data-stu-id="4a47a-182">If the database has been created, then `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) can be used to delete it.</span></span>

<span data-ttu-id="4a47a-183">Une fois qu’il est confirmé que la base de données n’existe pas, supprimez la migration initiale avec `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (.NET Core CLI).</span><span class="sxs-lookup"><span data-stu-id="4a47a-183">Once it is confirmed that the database doesn't exist,  remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>

<span data-ttu-id="4a47a-184">Mise à jour la `ApplicationDbContext` à une autre classe de base, en spécifiant le nouveau type de clé de `TKey`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-184">Update the `ApplicationDbContext` to use a different base class, specifying the new key type for `TKey`.</span></span> <span data-ttu-id="4a47a-185">Par exemple, pour utiliser un `Guid` clé :</span><span class="sxs-lookup"><span data-stu-id="4a47a-185">For example, to use a `Guid` key:</span></span>

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

<span data-ttu-id="4a47a-186">Notez que les classes génériques `IdentityUser<TKey>` et `IdentityRole<TKey>` doit également être spécifié à utiliser le nouveau type de clé.</span><span class="sxs-lookup"><span data-stu-id="4a47a-186">Notice that the generic classes `IdentityUser<TKey>` and `IdentityRole<TKey>` must also be specified to use the new key type.</span></span> <span data-ttu-id="4a47a-187">`ConfigureServices` doit être mis à jour pour utiliser l’utilisateur générique :</span><span class="sxs-lookup"><span data-stu-id="4a47a-187">`ConfigureServices` must be updated to use the generic user:</span></span>

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="4a47a-188">Si une personnalisée `ApplicationUser` est utilisé, mettre à jour qu’elle hérite `IdentityUser<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-188">If a custom `ApplicationUser` is being used, update it to inherit from `IdentityUser<TKey>`.</span></span> <span data-ttu-id="4a47a-189">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4a47a-189">For example:</span></span>

```CSharp
public class ApplicationUser : IdentityUser<Guid>
{
    public string CustomTag { get; set; }
}

public class ApplicationDbContext : IdentityDbContext<ApplicationUser, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="adding-navigation-properties"></a><span data-ttu-id="4a47a-190">Ajout de propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="4a47a-190">Adding navigation properties</span></span>

<span data-ttu-id="4a47a-191">Modification de la configuration de modèle pour les relations peut être un plus difficile que d’autres modifications apportées.</span><span class="sxs-lookup"><span data-stu-id="4a47a-191">Changing the model configuration for relationships can be a more difficult than making other changes.</span></span> <span data-ttu-id="4a47a-192">Veillez à remplacer les relations existantes au lieu de créer des relations supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="4a47a-192">Care must be taken to replace the existing relationships rather than create a new additional relationships.</span></span> <span data-ttu-id="4a47a-193">En particulier, la relation modifiée doit spécifier la même propriété de clé étrangère en tant que la relation existante.</span><span class="sxs-lookup"><span data-stu-id="4a47a-193">In particular, the changed relationship must specify the same foreign key property as the existing relationship.</span></span> <span data-ttu-id="4a47a-194">Par exemple, la relation entre `Users` et `UserClaims` est par défaut spécifié en tant que :</span><span class="sxs-lookup"><span data-stu-id="4a47a-194">For example, the relationship between `Users` and `UserClaims` is by default specified as:</span></span>

```CSharp
builder.Entity<TUser>(b =>
{
    // Each User can have many UserClaims
    b.HasMany<TUserClaim>()
     .WithOne()
     .HasForeignKey(uc => uc.UserId)
     .IsRequired();
});
```

<span data-ttu-id="4a47a-195">La clé étrangère pour cette relation est spécifiée en tant que le `UserClaim.UserId` propriété.</span><span class="sxs-lookup"><span data-stu-id="4a47a-195">The foreign key for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="4a47a-196">`HasMany` et `WithOne` sont appelés sans arguments pour créer la relation sans les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="4a47a-196">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="4a47a-197">Ajouter une propriété de navigation `ApplicationUser` qui permettra associés `UserClaims` être référencé à partir de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4a47a-197">Add a navigation property to `ApplicationUser` that will allow associated `UserClaims` to be referenced from the user:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="4a47a-198">Notez que la `TKey` pour `IdentityUserClaim<TKey>` est de type spécifié pour la clé primaire d’utilisateurs--dans ce cas `string` étant donné que nous utilisons les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="4a47a-198">Note that the `TKey` for `IdentityUserClaim<TKey>` is type specified for the primary key of users--in this case `string` since we're using the defaults.</span></span> <span data-ttu-id="4a47a-199">Il s’agit de **pas** le type de clé primaire pour la `UserClaim` type d’entité.</span><span class="sxs-lookup"><span data-stu-id="4a47a-199">It is **not** the primary key type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="4a47a-200">Maintenant que la propriété de navigation existe il doit être configuré dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="4a47a-200">Now that the navigation property exists it must be configured in `OnModelCreating`:</span></span>

```CSharp
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

<span data-ttu-id="4a47a-201">Notez que la relation est configurée tel qu’il était avant, uniquement avec une propriété de navigation spécifiée dans l’appel à `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-201">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="4a47a-202">Les propriétés de navigation existent uniquement dans le modèle EF, pas la base de données.</span><span class="sxs-lookup"><span data-stu-id="4a47a-202">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="4a47a-203">Étant donné que la clé étrangère de la relation n’a pas changé, ce type de modification de modèle ne requiert pas la base de données à mettre à jour.</span><span class="sxs-lookup"><span data-stu-id="4a47a-203">Because the foreign key for the relationship has not changed, this kind of model change does not require the database to be updated.</span></span> <span data-ttu-id="4a47a-204">Cela peut être vérifié en ajoutant une migration après avoir effectué la modification : le `Up` et `Down` méthodes sont vides.</span><span class="sxs-lookup"><span data-stu-id="4a47a-204">This can be checked by adding a migration after making the change: the `Up` and `Down` methods are empty.</span></span>

### <a name="adding-all-user-navigation-properties"></a><span data-ttu-id="4a47a-205">Ajout de toutes les propriétés de navigation utilisateur</span><span class="sxs-lookup"><span data-stu-id="4a47a-205">Adding all User navigation properties</span></span>

<span data-ttu-id="4a47a-206">À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation unidirectionnel pour toutes les relations utilisateur :</span><span class="sxs-lookup"><span data-stu-id="4a47a-206">Using the section above as guidance, here is an example that configures unidirectional navigation properties for all relationships on User:</span></span>

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
    public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
    public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
    public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; }
}
```

```CSharp
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

### <a name="adding-user-and-role-navigation-properties"></a><span data-ttu-id="4a47a-207">Ajout de l’utilisateur et le rôle des propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="4a47a-207">Adding User and Role navigation properties</span></span>

<span data-ttu-id="4a47a-208">À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation pour toutes les relations sur l’utilisateur et le rôle :</span><span class="sxs-lookup"><span data-stu-id="4a47a-208">Using the section above as guidance, here is an example that configures navigation properties for all relationships on User and Role:</span></span>

```CSharp
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

```CSharp
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

<span data-ttu-id="4a47a-209">Remarques :</span><span class="sxs-lookup"><span data-stu-id="4a47a-209">Notes:</span></span>

* <span data-ttu-id="4a47a-210">Cet exemple inclut également le `UserRole` joindre l’entité, qui est nécessaire pour naviguer jusqu'à la relation plusieurs-à-plusieurs à partir des utilisateurs aux rôles.</span><span class="sxs-lookup"><span data-stu-id="4a47a-210">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="4a47a-211">N’oubliez pas de modifier les types des propriétés de navigation pour refléter le fait que `ApplicationXxx` types sont maintenant utilisés à la place de `IdentityXxx` types.</span><span class="sxs-lookup"><span data-stu-id="4a47a-211">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="4a47a-212">N’oubliez pas d’utiliser le `ApplicationXxx` dans le modèle générique `ApplicationContext` définition.</span><span class="sxs-lookup"><span data-stu-id="4a47a-212">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="adding-all-navigation-properties"></a><span data-ttu-id="4a47a-213">Ajout de toutes les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="4a47a-213">Adding all navigation properties</span></span>

<span data-ttu-id="4a47a-214">À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation pour toutes les relations sur tous les types d’entités :</span><span class="sxs-lookup"><span data-stu-id="4a47a-214">Using the section above as guidance, here is an example that configures navigation properties for all relationships on all entity types:</span></span>

```CSharp
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

```CSharp
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

### <a name="using-composite-keys"></a><span data-ttu-id="4a47a-215">À l’aide de clés composites</span><span class="sxs-lookup"><span data-stu-id="4a47a-215">Using composite keys</span></span>

<span data-ttu-id="4a47a-216">Les sections précédentes ont démontré la modification du type de clé utilisée dans le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="4a47a-216">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="4a47a-217">Modification du modèle de clé d’identité pour utiliser des clés composites n’est pas pris en charge ou n’est pas recommandée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-217">Changing the Identity key model to use composite keys is not supported or recommended.</span></span> <span data-ttu-id="4a47a-218">À l’aide d’une clé composite avec l’identité impliquerait de modifier la façon dont le code de gestionnaire d’identité interagit avec le modèle, qui est une personnalisation non pris en charge et dépasse le cadre de ce document.</span><span class="sxs-lookup"><span data-stu-id="4a47a-218">Using a composite key with Identity would involve changing how the Identity manager code interacts with the model, which is an unsupported customization and beyond the scope of this document.</span></span>

### <a name="changing-tablecolumn-names-and-facets"></a><span data-ttu-id="4a47a-219">Modification des noms de colonne de la table et les facettes</span><span class="sxs-lookup"><span data-stu-id="4a47a-219">Changing table/column names and facets</span></span>

<span data-ttu-id="4a47a-220">Pour modifier les noms des tables et des colonnes, appelez `base.OnModelCreating`, puis ajoutez la configuration pour remplacer tous les paramètres par défaut.</span><span class="sxs-lookup"><span data-stu-id="4a47a-220">To change the names of tables and columns, call `base.OnModelCreating`, and then add configuration to override any of the defaults.</span></span> <span data-ttu-id="4a47a-221">Par exemple, pour modifier le nom de toutes les tables d’identité :</span><span class="sxs-lookup"><span data-stu-id="4a47a-221">For example, to change the name of all the identity tables:</span></span>

```CSharp
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

<span data-ttu-id="4a47a-222">Notez que ces exemples utilisent les types d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="4a47a-222">Note that these examples use the default Identity types.</span></span> <span data-ttu-id="4a47a-223">Si vous utilisez un type d’application, telle que `ApplicationUser` puis configurez ce type au lieu du type de valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="4a47a-223">If you are using an application type, such as `ApplicationUser` then configure that type instead of the default type.</span></span>

<span data-ttu-id="4a47a-224">Voici un exemple qui modifie certains noms de colonne :</span><span class="sxs-lookup"><span data-stu-id="4a47a-224">Here is an example that changes some column names:</span></span>

```CSharp
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

<span data-ttu-id="4a47a-225">Certains types de colonnes de la base de données peuvent être configurés avec certaines _facettes_ telles que la longueur de chaîne maximale autorisée.</span><span class="sxs-lookup"><span data-stu-id="4a47a-225">Some types of database columns can be configured with certain _facets_ such as the maximum string length allowed.</span></span> <span data-ttu-id="4a47a-226">Voici un exemple qui définit des longueurs de colonne maximale pour plusieurs propriétés de chaîne dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="4a47a-226">Here is an example that sets column max lengths for several string properties in the model:</span></span>

```CSharp
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

### <a name="mapping-to-a-different-schema"></a><span data-ttu-id="4a47a-227">Mappage vers un autre schéma</span><span class="sxs-lookup"><span data-stu-id="4a47a-227">Mapping to a different schema</span></span>

<span data-ttu-id="4a47a-228">Les schémas peuvent se comporter différemment dans les fournisseurs de base de données différente, mais pour SQL Server, la valeur par défaut consiste à créer de toutes les tables dans le schéma « dbo ».</span><span class="sxs-lookup"><span data-stu-id="4a47a-228">Schemas can behave differently in different database providers, but for SQL Server, the default is to create all tables in the "dbo" schema.</span></span> <span data-ttu-id="4a47a-229">Cela peut être modifié pour à la place créer les tables dans un schéma différent.</span><span class="sxs-lookup"><span data-stu-id="4a47a-229">This can be changed to instead create the tables in a different schema.</span></span> <span data-ttu-id="4a47a-230">Exemple :</span><span class="sxs-lookup"><span data-stu-id="4a47a-230">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a><span data-ttu-id="4a47a-231">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="4a47a-231">Lazy loading</span></span>

<span data-ttu-id="4a47a-232">Dans cette section prise en charge pour le chargement différé des proxys dans le modèle d’identité est ajouté.</span><span class="sxs-lookup"><span data-stu-id="4a47a-232">In this section support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="4a47a-233">Chargement lent est utile, car elle permet les propriétés de navigation à être utilisée sans la première s’assurer qu’ils sont chargés.</span><span class="sxs-lookup"><span data-stu-id="4a47a-233">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they are loaded.</span></span>

<span data-ttu-id="4a47a-234">Types d’entités est possible adaptées à chargement différé de plusieurs façons, comme décrit dans la [EF documentations](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="4a47a-234">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="4a47a-235">Par souci de simplicité, nous allons utiliser des proxys de chargement différé, ce qui nécessite :</span><span class="sxs-lookup"><span data-stu-id="4a47a-235">For simplicity, we will use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="4a47a-236">Installation de le [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span><span class="sxs-lookup"><span data-stu-id="4a47a-236">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="4a47a-237">Un appel à `.UseLazyLoadingProxies()` dans `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="4a47a-237">A call to `.UseLazyLoadingProxies()` inside `AddDbContext`.</span></span>
* <span data-ttu-id="4a47a-238">Types d’entités publiques avec les propriétés de navigation virtuel public.</span><span class="sxs-lookup"><span data-stu-id="4a47a-238">Public entity types with public virtual navigation properties.</span></span>

<span data-ttu-id="4a47a-239">Voici un exemple de l’appel de `.UseLazyLoadingProxies()`:</span><span class="sxs-lookup"><span data-stu-id="4a47a-239">Here is an example of calling `.UseLazyLoadingProxies()`:</span></span>

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="4a47a-240">Consultez les exemples ci-dessus pour ajouter des propriétés de navigation pour les types d’entités.</span><span class="sxs-lookup"><span data-stu-id="4a47a-240">Refer back to the examples above for adding navigation properties to the entity types.</span></span>
