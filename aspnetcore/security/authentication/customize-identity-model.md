---
title: Personnalisation du modèle d’identité dans ASP.NET Core
author: ajcvickers
description: Cet article explique comment personnaliser le modèle de données Entity Framework Core sous-jacent pour ASP.NET Core identité.
ms.author: avickers
ms.date: 07/01/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: f549fdff4a416b5fadcb2b1078b051bbab8e402e
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656078"
---
# <a name="identity-model-customization-in-aspnet-core"></a><span data-ttu-id="b22c8-103">Personnalisation du modèle d’identité dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b22c8-103">Identity model customization in ASP.NET Core</span></span>

<span data-ttu-id="b22c8-104">Par [Arthur Vickers](https://github.com/ajcvickers)</span><span class="sxs-lookup"><span data-stu-id="b22c8-104">By [Arthur Vickers](https://github.com/ajcvickers)</span></span>

<span data-ttu-id="b22c8-105">ASP.NET Core identité fournit une infrastructure pour la gestion et le stockage des comptes d’utilisateur dans les applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b22c8-105">ASP.NET Core Identity provides a framework for managing and storing user accounts in ASP.NET Core apps.</span></span> <span data-ttu-id="b22c8-106">L’identité est ajoutée à votre projet lorsque **des comptes d’utilisateur individuels** est sélectionné comme mécanisme d’authentification.</span><span class="sxs-lookup"><span data-stu-id="b22c8-106">Identity is added to your project when **Individual User Accounts** is selected as the authentication mechanism.</span></span> <span data-ttu-id="b22c8-107">Par défaut, l’identité utilise un modèle de données de base Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="b22c8-107">By default, Identity makes use of an Entity Framework (EF) Core data model.</span></span> <span data-ttu-id="b22c8-108">Cet article explique comment personnaliser le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="b22c8-108">This article describes how to customize the Identity model.</span></span>

## <a name="identity-and-ef-core-migrations"></a><span data-ttu-id="b22c8-109">Migrations d’identité et de EF Core</span><span class="sxs-lookup"><span data-stu-id="b22c8-109">Identity and EF Core Migrations</span></span>

<span data-ttu-id="b22c8-110">Avant d’examiner le modèle, il est utile de comprendre comment l’identité fonctionne avec [EF Core migrations](/ef/core/managing-schemas/migrations/) pour créer et mettre à jour une base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-110">Before examining the model, it's useful to understand how Identity works with [EF Core Migrations](/ef/core/managing-schemas/migrations/) to create and update a database.</span></span> <span data-ttu-id="b22c8-111">Au niveau supérieur, le processus est le suivant :</span><span class="sxs-lookup"><span data-stu-id="b22c8-111">At the top level, the process is:</span></span>

1. <span data-ttu-id="b22c8-112">Définir ou mettre à jour un [modèle de données dans le code](/ef/core/modeling/).</span><span class="sxs-lookup"><span data-stu-id="b22c8-112">Define or update a [data model in code](/ef/core/modeling/).</span></span>
1. <span data-ttu-id="b22c8-113">Ajoutez une migration pour traduire ce modèle en modifications qui peuvent être appliquées à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-113">Add a Migration to translate this model into changes that can be applied to the database.</span></span>
1. <span data-ttu-id="b22c8-114">Vérifiez que la migration représente correctement vos intentions.</span><span class="sxs-lookup"><span data-stu-id="b22c8-114">Check that the Migration correctly represents your intentions.</span></span>
1. <span data-ttu-id="b22c8-115">Appliquez la migration pour mettre à jour la base de données afin qu’elle soit synchronisée avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-115">Apply the Migration to update the database to be in sync with the model.</span></span>
1. <span data-ttu-id="b22c8-116">Répétez les étapes 1 à 4 pour affiner le modèle et maintenir la synchronisation de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-116">Repeat steps 1 through 4 to further refine the model and keep the database in sync.</span></span>

<span data-ttu-id="b22c8-117">Utilisez l’une des approches suivantes pour ajouter et appliquer des migrations :</span><span class="sxs-lookup"><span data-stu-id="b22c8-117">Use one of the following approaches to add and apply Migrations:</span></span>

* <span data-ttu-id="b22c8-118">La fenêtre **console du gestionnaire de package** (PMC) si vous utilisez Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b22c8-118">The **Package Manager Console** (PMC) window if using Visual Studio.</span></span> <span data-ttu-id="b22c8-119">Pour plus d’informations, consultez [EF Core les outils PMC](/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="b22c8-119">For more information, see [EF Core PMC tools](/ef/core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="b22c8-120">CLI .NET Core en cas d’utilisation de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="b22c8-120">The .NET Core CLI if using the command line.</span></span> <span data-ttu-id="b22c8-121">Pour plus d’informations, consultez [EF Core les outils en ligne de commande .net](/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="b22c8-121">For more information, see [EF Core .NET command line tools](/ef/core/miscellaneous/cli/dotnet).</span></span>
* <span data-ttu-id="b22c8-122">En cliquant sur le bouton **appliquer des migrations** sur la page d’erreur lors de l’exécution de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22c8-122">Clicking the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="b22c8-123">ASP.NET Core a un gestionnaire de page d’erreurs au moment du développement.</span><span class="sxs-lookup"><span data-stu-id="b22c8-123">ASP.NET Core has a development-time error page handler.</span></span> <span data-ttu-id="b22c8-124">Le gestionnaire peut appliquer des migrations lorsque l’application est exécutée.</span><span class="sxs-lookup"><span data-stu-id="b22c8-124">The handler can apply migrations when the app is run.</span></span> <span data-ttu-id="b22c8-125">Les applications de production génèrent généralement des scripts SQL à partir des migrations et déploient des modifications de base de données dans le cadre d’un déploiement d’application contrôlé et de base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-125">Production apps typically generate SQL scripts from the migrations and deploy database changes as part of a controlled app and database deployment.</span></span>

<span data-ttu-id="b22c8-126">Quand une nouvelle application utilisant l’identité est créée, les étapes 1 et 2 ci-dessus ont déjà été effectuées.</span><span class="sxs-lookup"><span data-stu-id="b22c8-126">When a new app using Identity is created, steps 1 and 2 above have already been completed.</span></span> <span data-ttu-id="b22c8-127">Autrement dit, le modèle de données initial existe déjà et la migration initiale a été ajoutée au projet.</span><span class="sxs-lookup"><span data-stu-id="b22c8-127">That is, the initial data model already exists, and the initial migration has been added to the project.</span></span> <span data-ttu-id="b22c8-128">La migration initiale doit toujours être appliquée à la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-128">The initial migration still needs to be applied to the database.</span></span> <span data-ttu-id="b22c8-129">La migration initiale peut être appliquée via l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22c8-129">The initial migration can be applied via one of the following approaches:</span></span>

* <span data-ttu-id="b22c8-130">Exécutez `Update-Database` dans PMC.</span><span class="sxs-lookup"><span data-stu-id="b22c8-130">Run `Update-Database` in PMC.</span></span>
* <span data-ttu-id="b22c8-131">Exécutez `dotnet ef database update` dans un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="b22c8-131">Run `dotnet ef database update` in a command shell.</span></span>
* <span data-ttu-id="b22c8-132">Cliquez sur le bouton **appliquer des migrations** sur la page d’erreur lors de l’exécution de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22c8-132">Click the **Apply Migrations** button on the error page when the app is run.</span></span>

<span data-ttu-id="b22c8-133">Répétez les étapes précédentes au fur et à mesure que des modifications sont apportées au modèle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-133">Repeat the preceding steps as changes are made to the model.</span></span>

## <a name="the-identity-model"></a><span data-ttu-id="b22c8-134">Modèle d’identité</span><span class="sxs-lookup"><span data-stu-id="b22c8-134">The Identity model</span></span>

### <a name="entity-types"></a><span data-ttu-id="b22c8-135">Types d’entités</span><span class="sxs-lookup"><span data-stu-id="b22c8-135">Entity types</span></span>

<span data-ttu-id="b22c8-136">Le modèle d’identité se compose des types d’entités suivants.</span><span class="sxs-lookup"><span data-stu-id="b22c8-136">The Identity model consists of the following entity types.</span></span>

|<span data-ttu-id="b22c8-137">Type d'entité</span><span class="sxs-lookup"><span data-stu-id="b22c8-137">Entity type</span></span>|<span data-ttu-id="b22c8-138">Description</span><span class="sxs-lookup"><span data-stu-id="b22c8-138">Description</span></span>                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |<span data-ttu-id="b22c8-139">Représente l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b22c8-139">Represents the user.</span></span>                                         |
|`Role`     |<span data-ttu-id="b22c8-140">Représente un rôle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-140">Represents a role.</span></span>                                           |
|`UserClaim`|<span data-ttu-id="b22c8-141">Représente une revendication qu’un utilisateur possède.</span><span class="sxs-lookup"><span data-stu-id="b22c8-141">Represents a claim that a user possesses.</span></span>                    |
|`UserToken`|<span data-ttu-id="b22c8-142">Représente un jeton d’authentification pour un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b22c8-142">Represents an authentication token for a user.</span></span>               |
|`UserLogin`|<span data-ttu-id="b22c8-143">Associe un utilisateur à une connexion.</span><span class="sxs-lookup"><span data-stu-id="b22c8-143">Associates a user with a login.</span></span>                              |
|`RoleClaim`|<span data-ttu-id="b22c8-144">Représente une revendication accordée à tous les utilisateurs d’un rôle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-144">Represents a claim that's granted to all users within a role.</span></span>|
|`UserRole` |<span data-ttu-id="b22c8-145">Entité de jointure qui associe les utilisateurs et les rôles.</span><span class="sxs-lookup"><span data-stu-id="b22c8-145">A join entity that associates users and roles.</span></span>               |

### <a name="entity-type-relationships"></a><span data-ttu-id="b22c8-146">Relations de type d’entité</span><span class="sxs-lookup"><span data-stu-id="b22c8-146">Entity type relationships</span></span>

<span data-ttu-id="b22c8-147">Les [types d’entités](#entity-types) sont liés les uns aux autres des manières suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22c8-147">The [entity types](#entity-types) are related to each other in the following ways:</span></span>

* <span data-ttu-id="b22c8-148">Chaque `User` peut avoir plusieurs `UserClaims`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-148">Each `User` can have many `UserClaims`.</span></span>
* <span data-ttu-id="b22c8-149">Chaque `User` peut avoir plusieurs `UserLogins`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-149">Each `User` can have many `UserLogins`.</span></span>
* <span data-ttu-id="b22c8-150">Chaque `User` peut avoir plusieurs `UserTokens`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-150">Each `User` can have many `UserTokens`.</span></span>
* <span data-ttu-id="b22c8-151">Chaque `Role` peut avoir de nombreux `RoleClaims`associés.</span><span class="sxs-lookup"><span data-stu-id="b22c8-151">Each `Role` can have many associated `RoleClaims`.</span></span>
* <span data-ttu-id="b22c8-152">Chaque `User` peut avoir de nombreux `Roles`associés, et chaque `Role` peut être associé à de nombreux `Users`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-152">Each `User` can have many associated `Roles`, and each `Role` can be associated with many `Users`.</span></span> <span data-ttu-id="b22c8-153">Il s’agit d’une relation plusieurs-à-plusieurs qui requiert une table de jointure dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-153">This is a many-to-many relationship that requires a join table in the database.</span></span> <span data-ttu-id="b22c8-154">La table de jointure est représentée par l’entité `UserRole`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-154">The join table is represented by the `UserRole` entity.</span></span>

### <a name="default-model-configuration"></a><span data-ttu-id="b22c8-155">Configuration de modèle par défaut</span><span class="sxs-lookup"><span data-stu-id="b22c8-155">Default model configuration</span></span>

<span data-ttu-id="b22c8-156">L’identité définit de nombreuses *classes de contexte* qui héritent de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) pour configurer et utiliser le modèle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-156">Identity defines many *context classes* that inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) to configure and use the model.</span></span> <span data-ttu-id="b22c8-157">Cette configuration s’effectue à l’aide de l' [API EF Core code First Fluent](/ef/core/modeling/) dans la méthode [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) de la classe Context.</span><span class="sxs-lookup"><span data-stu-id="b22c8-157">This configuration is done using the [EF Core Code First Fluent API](/ef/core/modeling/) in the [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating) method of the context class.</span></span> <span data-ttu-id="b22c8-158">La configuration par défaut est la suivante :</span><span class="sxs-lookup"><span data-stu-id="b22c8-158">The default configuration is:</span></span>

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

### <a name="model-generic-types"></a><span data-ttu-id="b22c8-159">Types génériques de modèle</span><span class="sxs-lookup"><span data-stu-id="b22c8-159">Model generic types</span></span>

<span data-ttu-id="b22c8-160">L’identité définit les types CLR ( [Common Language Runtime](/dotnet/standard/glossary#clr) ) par défaut pour chacun des types d’entité listés ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b22c8-160">Identity defines default [Common Language Runtime](/dotnet/standard/glossary#clr) (CLR) types for each of the entity types listed above.</span></span> <span data-ttu-id="b22c8-161">Tous les types portent le préfixe *Identity*:</span><span class="sxs-lookup"><span data-stu-id="b22c8-161">These types are all prefixed with *Identity*:</span></span>

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

<span data-ttu-id="b22c8-162">Au lieu d’utiliser directement ces types, les types peuvent être utilisés comme classes de base pour les types de l’application.</span><span class="sxs-lookup"><span data-stu-id="b22c8-162">Rather than using these types directly, the types can be used as base classes for the app's own types.</span></span> <span data-ttu-id="b22c8-163">Les classes `DbContext` définies par l’identité sont génériques, de telle sorte que différents types CLR peuvent être utilisés pour un ou plusieurs types d’entité dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-163">The `DbContext` classes defined by Identity are generic, such that different CLR types can be used for one or more of the entity types in the model.</span></span> <span data-ttu-id="b22c8-164">Ces types génériques autorisent également la modification du type de données `User` clé primaire (PK).</span><span class="sxs-lookup"><span data-stu-id="b22c8-164">These generic types also allow the `User` primary key (PK) data type to be changed.</span></span>

<span data-ttu-id="b22c8-165">Lors de l’utilisation de l’identité avec la prise en charge des rôles, une classe <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> doit être utilisée.</span><span class="sxs-lookup"><span data-stu-id="b22c8-165">When using Identity with support for roles, an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> class should be used.</span></span> <span data-ttu-id="b22c8-166">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b22c8-166">For example:</span></span>

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

<span data-ttu-id="b22c8-167">Il est également possible d’utiliser l’identité sans rôles (uniquement les revendications), auquel cas une classe <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> doit être utilisée :</span><span class="sxs-lookup"><span data-stu-id="b22c8-167">It's also possible to use Identity without roles (only claims), in which case an <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> class should be used:</span></span>

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

## <a name="customize-the-model"></a><span data-ttu-id="b22c8-168">Personnaliser le modèle</span><span class="sxs-lookup"><span data-stu-id="b22c8-168">Customize the model</span></span>

<span data-ttu-id="b22c8-169">Le point de départ pour la personnalisation de modèle consiste à dériver du type de contexte approprié.</span><span class="sxs-lookup"><span data-stu-id="b22c8-169">The starting point for model customization is to derive from the appropriate context type.</span></span> <span data-ttu-id="b22c8-170">Consultez la section [types génériques de modèle](#model-generic-types) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-170">See the [Model generic types](#model-generic-types) section.</span></span> <span data-ttu-id="b22c8-171">Ce type de contexte est habituellement appelé `ApplicationDbContext` et est créé par les modèles de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b22c8-171">This context type is customarily called `ApplicationDbContext` and is created by the ASP.NET Core templates.</span></span>

<span data-ttu-id="b22c8-172">Le contexte est utilisé pour configurer le modèle de deux manières :</span><span class="sxs-lookup"><span data-stu-id="b22c8-172">The context is used to configure the model in two ways:</span></span>

* <span data-ttu-id="b22c8-173">Fourniture des types d’entité et de clé pour les paramètres de type générique.</span><span class="sxs-lookup"><span data-stu-id="b22c8-173">Supplying entity and key types for the generic type parameters.</span></span>
* <span data-ttu-id="b22c8-174">Substitution de `OnModelCreating` pour modifier le mappage de ces types.</span><span class="sxs-lookup"><span data-stu-id="b22c8-174">Overriding `OnModelCreating` to modify the mapping of these types.</span></span>

<span data-ttu-id="b22c8-175">Lors de la substitution de `OnModelCreating`, `base.OnModelCreating` doit être appelé en premier ; la configuration de substitution doit être appelée Next.</span><span class="sxs-lookup"><span data-stu-id="b22c8-175">When overriding `OnModelCreating`, `base.OnModelCreating` should be called first; the overriding configuration should be called next.</span></span> <span data-ttu-id="b22c8-176">EF Core a généralement une stratégie dernière-un WINS pour la configuration.</span><span class="sxs-lookup"><span data-stu-id="b22c8-176">EF Core generally has a last-one-wins policy for configuration.</span></span> <span data-ttu-id="b22c8-177">Par exemple, si la méthode `ToTable` pour un type d’entité est appelée en premier avec un nom de table, puis à nouveau ultérieurement avec un autre nom de table, le nom de la table dans le deuxième appel est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b22c8-177">For example, if the `ToTable` method for an entity type is called first with one table name and then again later with a different table name, the table name in the second call is used.</span></span>

### <a name="custom-user-data"></a><span data-ttu-id="b22c8-178">Données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="b22c8-178">Custom user data</span></span>

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

<span data-ttu-id="b22c8-179">Les [données utilisateur personnalisées](xref:security/authentication/add-user-data) sont prises en charge en héritant de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-179">[Custom user data](xref:security/authentication/add-user-data) is supported by inheriting from `IdentityUser`.</span></span> <span data-ttu-id="b22c8-180">Il est personnalisé pour nommer ce type `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-180">It's customary to name this type `ApplicationUser`:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

<span data-ttu-id="b22c8-181">Utilisez le type de `ApplicationUser` comme argument générique pour le contexte :</span><span class="sxs-lookup"><span data-stu-id="b22c8-181">Use the `ApplicationUser` type as a generic argument for the context:</span></span>

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
    }
}
```

<span data-ttu-id="b22c8-182">Il n’est pas nécessaire de remplacer `OnModelCreating` dans la classe `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-182">There's no need to override `OnModelCreating` in the `ApplicationDbContext` class.</span></span> <span data-ttu-id="b22c8-183">EF Core mappe la propriété `CustomTag` par Convention.</span><span class="sxs-lookup"><span data-stu-id="b22c8-183">EF Core maps the `CustomTag` property by convention.</span></span> <span data-ttu-id="b22c8-184">Toutefois, la base de données doit être mise à jour pour créer une colonne de `CustomTag`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-184">However, the database needs to be updated to create a new `CustomTag` column.</span></span> <span data-ttu-id="b22c8-185">Pour créer la colonne, ajoutez une migration, puis mettez à jour la base de données comme décrit dans la rubrique [migrations d’identité et de EF Core](#identity-and-ef-core-migrations).</span><span class="sxs-lookup"><span data-stu-id="b22c8-185">To create the column, add a migration, and then update the database as described in [Identity and EF Core Migrations](#identity-and-ef-core-migrations).</span></span>

<span data-ttu-id="b22c8-186">Mettez à jour *pages/Shared/_LoginPartial. cshtml* et remplacez `IdentityUser` par `ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-186">Update *Pages/Shared/_LoginPartial.cshtml* and replace `IdentityUser` with `ApplicationUser`:</span></span>

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

<span data-ttu-id="b22c8-187">Mettez à jour *Areas/Identity/IdentityHostingStartup. cs* ou `Startup.ConfigureServices` et remplacez `IdentityUser` par `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-187">Update *Areas/Identity/IdentityHostingStartup.cs*  or `Startup.ConfigureServices` and replace `IdentityUser` with `ApplicationUser`.</span></span>

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

<span data-ttu-id="b22c8-188">Dans ASP.NET Core 2,1 ou version ultérieure, l’identité est fournie sous la forme d’une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="b22c8-188">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="b22c8-189">Pour plus d’informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-189">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="b22c8-190">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-190">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="b22c8-191">Si le générateur de modèles d’identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-191">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span> <span data-ttu-id="b22c8-192">Pour plus d'informations, consultez les pages suivantes :</span><span class="sxs-lookup"><span data-stu-id="b22c8-192">For more information, see:</span></span>

* [<span data-ttu-id="b22c8-193">Ajouter un modèle automatique d’identité</span><span class="sxs-lookup"><span data-stu-id="b22c8-193">Scaffold Identity</span></span>](xref:security/authentication/scaffold-identity)
* [<span data-ttu-id="b22c8-194">Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité</span><span class="sxs-lookup"><span data-stu-id="b22c8-194">Add, download, and delete custom user data to Identity</span></span>](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a><span data-ttu-id="b22c8-195">Modifier le type de clé primaire</span><span class="sxs-lookup"><span data-stu-id="b22c8-195">Change the primary key type</span></span>

<span data-ttu-id="b22c8-196">Une modification du type de données de la colonne de clé primaire après la création de la base de données est problématique sur de nombreux systèmes de base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-196">A change to the PK column's data type after the database has been created is problematic on many database systems.</span></span> <span data-ttu-id="b22c8-197">La modification de la clé primaire implique généralement la suppression et la recréation de la table.</span><span class="sxs-lookup"><span data-stu-id="b22c8-197">Changing the PK typically involves dropping and re-creating the table.</span></span> <span data-ttu-id="b22c8-198">Par conséquent, les types de clés doivent être spécifiés lors de la migration initiale lors de la création de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-198">Therefore, key types should be specified in the initial migration when the database is created.</span></span>

<span data-ttu-id="b22c8-199">Pour modifier le type de clé primaire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="b22c8-199">Follow these steps to change the PK type:</span></span>

1. <span data-ttu-id="b22c8-200">Si la base de données a été créée avant la modification de la clé primaire, exécutez `Drop-Database` (PMC) ou `dotnet ef database drop` (CLI .NET Core) pour la supprimer.</span><span class="sxs-lookup"><span data-stu-id="b22c8-200">If the database was created before the PK change, run `Drop-Database` (PMC) or `dotnet ef database drop` (.NET Core CLI) to delete it.</span></span>
2. <span data-ttu-id="b22c8-201">Après confirmation de la suppression de la base de données, supprimez la migration initiale avec `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (CLI .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b22c8-201">After confirming deletion of the database, remove the initial migration with `Remove-Migration` (PMC) or `dotnet ef migrations remove` (.NET Core CLI).</span></span>
3. <span data-ttu-id="b22c8-202">Mettez à jour la classe `ApplicationDbContext` à dériver de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-202">Update the `ApplicationDbContext` class to derive from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>.</span></span> <span data-ttu-id="b22c8-203">Spécifiez le nouveau type de clé pour `TKey`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-203">Specify the new key type for `TKey`.</span></span> <span data-ttu-id="b22c8-204">Par exemple, pour utiliser un type de clé `Guid` :</span><span class="sxs-lookup"><span data-stu-id="b22c8-204">For example, to use a `Guid` key type:</span></span>

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

    <span data-ttu-id="b22c8-205">Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> doivent être spécifiées pour utiliser le nouveau type de clé.</span><span class="sxs-lookup"><span data-stu-id="b22c8-205">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    <span data-ttu-id="b22c8-206">Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> doivent être spécifiées pour utiliser le nouveau type de clé.</span><span class="sxs-lookup"><span data-stu-id="b22c8-206">In the preceding code, the generic classes <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> and <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> must be specified to use the new key type.</span></span>

    ::: moniker-end

    <span data-ttu-id="b22c8-207">`Startup.ConfigureServices` doit être mis à jour pour utiliser l’utilisateur générique :</span><span class="sxs-lookup"><span data-stu-id="b22c8-207">`Startup.ConfigureServices` must be updated to use the generic user:</span></span>

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

4. <span data-ttu-id="b22c8-208">Si une classe de `ApplicationUser` personnalisée est utilisée, mettez à jour la classe pour qu’elle hérite de `IdentityUser`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-208">If a custom `ApplicationUser` class is being used, update the class to inherit from `IdentityUser`.</span></span> <span data-ttu-id="b22c8-209">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b22c8-209">For example:</span></span>

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    <span data-ttu-id="b22c8-210">Mettez à jour `ApplicationDbContext` pour référencer la classe de `ApplicationUser` personnalisée :</span><span class="sxs-lookup"><span data-stu-id="b22c8-210">Update `ApplicationDbContext` to reference the custom `ApplicationUser` class:</span></span>

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

    <span data-ttu-id="b22c8-211">Inscrivez la classe de contexte de base de données personnalisée lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-211">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="b22c8-212">Le type de données de la clé primaire est déduit en analysant l’objet [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-212">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="b22c8-213">Dans ASP.NET Core 2,1 ou version ultérieure, l’identité est fournie sous la forme d’une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="b22c8-213">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="b22c8-214">Pour plus d’informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-214">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="b22c8-215">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-215">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="b22c8-216">Si le générateur de modèles d’identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-216">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="b22c8-217">Le type de données de la clé primaire est déduit en analysant l’objet [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-217">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    <span data-ttu-id="b22c8-218">La méthode <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> accepte un type de `TKey` qui indique le type de données de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b22c8-218">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

5. <span data-ttu-id="b22c8-219">Si une classe de `ApplicationRole` personnalisée est utilisée, mettez à jour la classe pour qu’elle hérite de `IdentityRole<TKey>`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-219">If a custom `ApplicationRole` class is being used, update the class to inherit from `IdentityRole<TKey>`.</span></span> <span data-ttu-id="b22c8-220">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b22c8-220">For example:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    <span data-ttu-id="b22c8-221">Mettez à jour `ApplicationDbContext` pour référencer la classe `ApplicationRole` personnalisée.</span><span class="sxs-lookup"><span data-stu-id="b22c8-221">Update `ApplicationDbContext` to reference the custom `ApplicationRole` class.</span></span> <span data-ttu-id="b22c8-222">Par exemple, la classe suivante référence une `ApplicationUser` personnalisée et une `ApplicationRole`personnalisée :</span><span class="sxs-lookup"><span data-stu-id="b22c8-222">For example, the following class references a custom `ApplicationUser` and a custom `ApplicationRole`:</span></span>

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="b22c8-223">Inscrivez la classe de contexte de base de données personnalisée lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-223">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    <span data-ttu-id="b22c8-224">Le type de données de la clé primaire est déduit en analysant l’objet [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-224">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    <span data-ttu-id="b22c8-225">Dans ASP.NET Core 2,1 ou version ultérieure, l’identité est fournie sous la forme d’une bibliothèque de classes Razor.</span><span class="sxs-lookup"><span data-stu-id="b22c8-225">In ASP.NET Core 2.1 or later, Identity is provided as a Razor Class Library.</span></span> <span data-ttu-id="b22c8-226">Pour plus d’informations, consultez <xref:security/authentication/scaffold-identity>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-226">For more information, see <xref:security/authentication/scaffold-identity>.</span></span> <span data-ttu-id="b22c8-227">Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span><span class="sxs-lookup"><span data-stu-id="b22c8-227">Consequently, the preceding code requires a call to <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>.</span></span> <span data-ttu-id="b22c8-228">Si le générateur de modèles d’identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-228">If the Identity scaffolder was used to add Identity files to the project, remove the call to `AddDefaultUI`.</span></span>

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="b22c8-229">Inscrivez la classe de contexte de base de données personnalisée lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-229">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="b22c8-230">Le type de données de la clé primaire est déduit en analysant l’objet [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-230">The primary key's data type is inferred by analyzing the [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) object.</span></span>

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    <span data-ttu-id="b22c8-231">Inscrivez la classe de contexte de base de données personnalisée lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-231">Register the custom database context class when adding the Identity service in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    <span data-ttu-id="b22c8-232">La méthode <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> accepte un type de `TKey` qui indique le type de données de la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b22c8-232">The <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> method accepts a `TKey` type indicating the primary key's data type.</span></span>

    ::: moniker-end

### <a name="add-navigation-properties"></a><span data-ttu-id="b22c8-233">Ajouter des propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="b22c8-233">Add navigation properties</span></span>

<span data-ttu-id="b22c8-234">La modification de la configuration du modèle pour les relations peut être plus difficile que d’apporter d’autres modifications.</span><span class="sxs-lookup"><span data-stu-id="b22c8-234">Changing the model configuration for relationships can be more difficult than making other changes.</span></span> <span data-ttu-id="b22c8-235">Il faut veiller à remplacer les relations existantes plutôt qu’à créer de nouvelles relations.</span><span class="sxs-lookup"><span data-stu-id="b22c8-235">Care must be taken to replace the existing relationships rather than create new, additional relationships.</span></span> <span data-ttu-id="b22c8-236">En particulier, la relation modifiée doit spécifier la même propriété de clé étrangère (FK) que la relation existante.</span><span class="sxs-lookup"><span data-stu-id="b22c8-236">In particular, the changed relationship must specify the same foreign key (FK) property as the existing relationship.</span></span> <span data-ttu-id="b22c8-237">Par exemple, la relation entre `Users` et `UserClaims` est, par défaut, spécifiée comme suit :</span><span class="sxs-lookup"><span data-stu-id="b22c8-237">For example, the relationship between `Users` and `UserClaims` is, by default, specified as follows:</span></span>

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

<span data-ttu-id="b22c8-238">Le FK pour cette relation est spécifié en tant que propriété `UserClaim.UserId`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-238">The FK for this relationship is specified as the `UserClaim.UserId` property.</span></span> <span data-ttu-id="b22c8-239">`HasMany` et `WithOne` sont appelées sans arguments pour créer la relation sans propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b22c8-239">`HasMany` and `WithOne` are called without arguments to create the relationship without navigation properties.</span></span>

<span data-ttu-id="b22c8-240">Ajoutez une propriété de navigation à `ApplicationUser` qui permet aux `UserClaims` associés d’être référencés à partir de l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="b22c8-240">Add a navigation property to `ApplicationUser` that allows associated `UserClaims` to be referenced from the user:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

<span data-ttu-id="b22c8-241">Le `TKey` pour `IdentityUserClaim<TKey>` est le type spécifié pour le PK des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b22c8-241">The `TKey` for `IdentityUserClaim<TKey>` is the type specified for the PK of users.</span></span> <span data-ttu-id="b22c8-242">Dans ce cas, `TKey` est `string` car les valeurs par défaut sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="b22c8-242">In this case, `TKey` is `string` because the defaults are being used.</span></span> <span data-ttu-id="b22c8-243">Il ne s’agit **pas** du type de clé primaire pour le type d’entité `UserClaim`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-243">It's **not** the PK type for the `UserClaim` entity type.</span></span>

<span data-ttu-id="b22c8-244">Maintenant que la propriété de navigation existe, elle doit être configurée dans `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-244">Now that the navigation property exists, it must be configured in `OnModelCreating`:</span></span>

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

<span data-ttu-id="b22c8-245">Notez que la relation est configurée exactement telle qu’elle était avant, uniquement avec une propriété de navigation spécifiée dans l’appel à `HasMany`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-245">Notice that relationship is configured exactly as it was before, only with a navigation property specified in the call to `HasMany`.</span></span>

<span data-ttu-id="b22c8-246">Les propriétés de navigation existent uniquement dans le modèle EF, pas dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-246">The navigation properties only exist in the EF model, not the database.</span></span> <span data-ttu-id="b22c8-247">Étant donné que le FK de la relation n’a pas changé, ce type de modification du modèle n’exige pas la mise à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-247">Because the FK for the relationship hasn't changed, this kind of model change doesn't require the database to be updated.</span></span> <span data-ttu-id="b22c8-248">Vous pouvez vérifier cela en ajoutant une migration après avoir apporté la modification.</span><span class="sxs-lookup"><span data-stu-id="b22c8-248">This can be checked by adding a migration after making the change.</span></span> <span data-ttu-id="b22c8-249">Les méthodes `Up` et `Down` sont vides.</span><span class="sxs-lookup"><span data-stu-id="b22c8-249">The `Up` and `Down` methods are empty.</span></span>

### <a name="add-all-user-navigation-properties"></a><span data-ttu-id="b22c8-250">Ajouter toutes les propriétés de navigation de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b22c8-250">Add all User navigation properties</span></span>

<span data-ttu-id="b22c8-251">À l’aide de la section ci-dessus, comme aide, l’exemple suivant configure les propriétés de navigation unidirectionnelles pour toutes les relations sur l’utilisateur :</span><span class="sxs-lookup"><span data-stu-id="b22c8-251">Using the section above as guidance, the following example configures unidirectional navigation properties for all relationships on User:</span></span>

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

### <a name="add-user-and-role-navigation-properties"></a><span data-ttu-id="b22c8-252">Ajouter des propriétés de navigation utilisateur et de rôle</span><span class="sxs-lookup"><span data-stu-id="b22c8-252">Add User and Role navigation properties</span></span>

<span data-ttu-id="b22c8-253">À l’aide de la section ci-dessus, en guise d’aide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur l’utilisateur et le rôle :</span><span class="sxs-lookup"><span data-stu-id="b22c8-253">Using the section above as guidance, the following example configures navigation properties for all relationships on User and Role:</span></span>

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

<span data-ttu-id="b22c8-254">Remarques :</span><span class="sxs-lookup"><span data-stu-id="b22c8-254">Notes:</span></span>

* <span data-ttu-id="b22c8-255">Cet exemple comprend également l’entité `UserRole` Join, qui est nécessaire pour parcourir la relation plusieurs-à-plusieurs des utilisateurs aux rôles.</span><span class="sxs-lookup"><span data-stu-id="b22c8-255">This example also includes the `UserRole` join entity, which is needed to navigate the many-to-many relationship from Users to Roles.</span></span>
* <span data-ttu-id="b22c8-256">N’oubliez pas de modifier les types des propriétés de navigation pour refléter le fait que les types de `ApplicationXxx` sont désormais utilisés à la place des types de `IdentityXxx`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-256">Remember to change the types of the navigation properties to reflect that `ApplicationXxx` types are now being used instead of `IdentityXxx` types.</span></span>
* <span data-ttu-id="b22c8-257">N’oubliez pas d’utiliser la `ApplicationXxx` dans la définition de `ApplicationContext` générique.</span><span class="sxs-lookup"><span data-stu-id="b22c8-257">Remember to use the `ApplicationXxx` in the generic `ApplicationContext` definition.</span></span>

### <a name="add-all-navigation-properties"></a><span data-ttu-id="b22c8-258">Ajouter toutes les propriétés de navigation</span><span class="sxs-lookup"><span data-stu-id="b22c8-258">Add all navigation properties</span></span>

<span data-ttu-id="b22c8-259">À l’aide de la section ci-dessus, en guise d’aide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur tous les types d’entité :</span><span class="sxs-lookup"><span data-stu-id="b22c8-259">Using the section above as guidance, the following example configures navigation properties for all relationships on all entity types:</span></span>

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

### <a name="use-composite-keys"></a><span data-ttu-id="b22c8-260">Utiliser des clés composites</span><span class="sxs-lookup"><span data-stu-id="b22c8-260">Use composite keys</span></span>

<span data-ttu-id="b22c8-261">Les sections précédentes ont démontré la modification du type de clé utilisé dans le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="b22c8-261">The preceding sections demonstrated changing the type of key used in the Identity model.</span></span> <span data-ttu-id="b22c8-262">La modification du modèle de clé d’identité pour utiliser des clés composites n’est pas prise en charge ou recommandée.</span><span class="sxs-lookup"><span data-stu-id="b22c8-262">Changing the Identity key model to use composite keys isn't supported or recommended.</span></span> <span data-ttu-id="b22c8-263">L’utilisation d’une clé composite avec l’identité implique de modifier la façon dont le code Identity Manager interagit avec le modèle.</span><span class="sxs-lookup"><span data-stu-id="b22c8-263">Using a composite key with Identity involves changing how the Identity manager code interacts with the model.</span></span> <span data-ttu-id="b22c8-264">Cette personnalisation n’est pas traitée dans ce document.</span><span class="sxs-lookup"><span data-stu-id="b22c8-264">This customization is beyond the scope of this document.</span></span>

### <a name="change-tablecolumn-names-and-facets"></a><span data-ttu-id="b22c8-265">Modifier les facettes et les noms de table/colonne</span><span class="sxs-lookup"><span data-stu-id="b22c8-265">Change table/column names and facets</span></span>

<span data-ttu-id="b22c8-266">Pour modifier les noms des tables et des colonnes, appelez `base.OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="b22c8-266">To change the names of tables and columns, call `base.OnModelCreating`.</span></span> <span data-ttu-id="b22c8-267">Ensuite, ajoutez une configuration pour remplacer les valeurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="b22c8-267">Then, add configuration to override any of the defaults.</span></span> <span data-ttu-id="b22c8-268">Par exemple, pour modifier le nom de toutes les tables d’identité :</span><span class="sxs-lookup"><span data-stu-id="b22c8-268">For example, to change the name of all the Identity tables:</span></span>

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

<span data-ttu-id="b22c8-269">Ces exemples utilisent les types d’identité par défaut.</span><span class="sxs-lookup"><span data-stu-id="b22c8-269">These examples use the default Identity types.</span></span> <span data-ttu-id="b22c8-270">Si vous utilisez un type d’application tel que `ApplicationUser`, configurez ce type à la place du type par défaut.</span><span class="sxs-lookup"><span data-stu-id="b22c8-270">If using an app type such as `ApplicationUser`, configure that type instead of the default type.</span></span>

<span data-ttu-id="b22c8-271">L’exemple suivant modifie certains noms de colonne :</span><span class="sxs-lookup"><span data-stu-id="b22c8-271">The following example changes some column names:</span></span>

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

<span data-ttu-id="b22c8-272">Certains types de colonnes de base de données peuvent être configurés avec certaines *facettes* (par exemple, la longueur maximale `string` autorisée).</span><span class="sxs-lookup"><span data-stu-id="b22c8-272">Some types of database columns can be configured with certain *facets* (for example, the maximum `string` length allowed).</span></span> <span data-ttu-id="b22c8-273">L’exemple suivant définit les longueurs maximales des colonnes pour plusieurs propriétés de `string` dans le modèle :</span><span class="sxs-lookup"><span data-stu-id="b22c8-273">The following example sets column maximum lengths for several `string` properties in the model:</span></span>

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

### <a name="map-to-a-different-schema"></a><span data-ttu-id="b22c8-274">Mapper à un schéma différent</span><span class="sxs-lookup"><span data-stu-id="b22c8-274">Map to a different schema</span></span>

<span data-ttu-id="b22c8-275">Les schémas peuvent se comporter différemment entre les fournisseurs de base de données.</span><span class="sxs-lookup"><span data-stu-id="b22c8-275">Schemas can behave differently across database providers.</span></span> <span data-ttu-id="b22c8-276">Par SQL Server, la valeur par défaut consiste à créer toutes les tables dans le schéma *dbo* .</span><span class="sxs-lookup"><span data-stu-id="b22c8-276">For SQL Server, the default is to create all tables in the *dbo* schema.</span></span> <span data-ttu-id="b22c8-277">Les tables peuvent être créées dans un schéma différent.</span><span class="sxs-lookup"><span data-stu-id="b22c8-277">The tables can be created in a different schema.</span></span> <span data-ttu-id="b22c8-278">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="b22c8-278">For example:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a><span data-ttu-id="b22c8-279">Chargement différé</span><span class="sxs-lookup"><span data-stu-id="b22c8-279">Lazy loading</span></span>

<span data-ttu-id="b22c8-280">Dans cette section, vous ajoutez la prise en charge des proxies de chargement différé dans le modèle d’identité.</span><span class="sxs-lookup"><span data-stu-id="b22c8-280">In this section, support for lazy-loading proxies in the Identity model is added.</span></span> <span data-ttu-id="b22c8-281">Le chargement différé est utile, car il permet d’utiliser les propriétés de navigation sans s’assurer d’abord qu’elles sont chargées.</span><span class="sxs-lookup"><span data-stu-id="b22c8-281">Lazy-loading is useful since it allows navigation properties to be used without first ensuring they're loaded.</span></span>

<span data-ttu-id="b22c8-282">Les types d’entités peuvent être adaptés au chargement différé de plusieurs façons, comme décrit dans la [documentation EF Core](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="b22c8-282">Entity types can be made suitable for lazy-loading in several ways, as described in the [EF Core documentation](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="b22c8-283">Pour plus de simplicité, utilisez des proxies à chargement différé, qui requièrent les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="b22c8-283">For simplicity, use lazy-loading proxies, which requires:</span></span>

* <span data-ttu-id="b22c8-284">Installation du package [Microsoft. EntityFrameworkCore. proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) .</span><span class="sxs-lookup"><span data-stu-id="b22c8-284">Installation of the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.</span></span>
* <span data-ttu-id="b22c8-285">Un appel à <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> à l’intérieur de [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="b22c8-285">A call to <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> inside [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span>
* <span data-ttu-id="b22c8-286">Types d’entités publics avec `public virtual` propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="b22c8-286">Public entity types with `public virtual` navigation properties.</span></span>

<span data-ttu-id="b22c8-287">L’exemple suivant illustre l’appel de `UseLazyLoadingProxies` dans `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b22c8-287">The following example demonstrates calling `UseLazyLoadingProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

<span data-ttu-id="b22c8-288">Reportez-vous aux exemples précédents pour obtenir des conseils sur l’ajout de propriétés de navigation aux types d’entités.</span><span class="sxs-lookup"><span data-stu-id="b22c8-288">Refer to the preceding examples for guidance on adding navigation properties to the entity types.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b22c8-289">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b22c8-289">Additional resources</span></span>

* <xref:security/authentication/scaffold-identity>

::: moniker-end
