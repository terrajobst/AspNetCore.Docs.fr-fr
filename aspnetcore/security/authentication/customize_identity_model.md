---
title: Personnalisation de modèle d’identité
author: ajcvickers
description: Cet article décrit comment personnaliser le modèle de données Entity Framework Core sous-jacent pour ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: avickers
ms.date: 04/12/2018
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 41ea125414c5997ee36f4e312beba4ff318a4a8d
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077675"
---
# <a name="identity-model-customization"></a>Personnalisation de modèle d’identité

Par [Arthur Vickers](https://github.com/ajcvickers)

Identité de ASP.NET Core fournit une infrastructure pour la gestion et le stockage des comptes d’utilisateur dans les applications ASP.NET Core. Identité est ajoutée à votre projet lorsque « Comptes d’utilisateur individuels » est sélectionné comme mécanisme d’authentification. Par défaut, identité utilise d’un Entity Framework (EF) modèle de données principal. Cet article décrit comment personnaliser le modèle d’identité.

<a name="identity-migrations"></a>

## <a name="identity-and-ef-core-migrations"></a>Identité et les Migrations de noyaux EF

Avant d’examiner le modèle, il est utile de comprendre le fonctionne d’identité avec [EF Core Migrations](/ef/core/managing-schemas/migrations/) pour créer et mettre à jour une base de données. Au niveau supérieur, le processus est :

1. Définir ou mettre à jour un [modèle de données dans le code](/ef/core/modeling/).
1. Ajoutez une migration pour ce modèle se traduire par les modifications qui peuvent être appliquées à la base de données.
1. Vérifiez que la migration représente correctement vos intentions.
1. Appliquer la migration pour mettre à jour la base de données pour être synchronisé avec le modèle.
1. Répétez les étapes 1 à 4 pour affiner le modèle et synchroniser la base de données.

Les migrations sont ajoutées et appliquées à l’aide de :

* Le [Package Manager Console](/ef/core/miscellaneous/cli/powershell) (PMC) si vous utilisez Visual Studio.
* Le [dotnet commandes](/ef/core/miscellaneous/cli/dotnet) sur la ligne de commande.
* En sélectionnant le lien de migrations sur la page d’erreur lors de l’application est exécutée.

ASP.NET Core a un gestionnaire de page d’erreur de délai de développement qui peut être utilisé pour appliquer des migrations lorsque l’application est exécutée. Pour les applications de production, il est souvent plus approprié pour générer des scripts SQL à partir de la migration et les déployer à la base de données dans le cadre d’un déploiement d’application et de base de données contrôlé.

Lorsqu’une nouvelle application à l’aide d’identité est créée, les étapes 1 et 2 ci-dessus ont déjà été effectuées. Autrement dit, le modèle de données initiale existe déjà, et la migration initiale a été ajoutée au projet. La migration initiale doit toujours être appliqué à la base de données. La migration initiale peut être effectuée en exécutant la base de données de mise à jour (PMC), la commande de mise à jour de (.NET Core CLI) dotnet ef de base de données, ou à l’aide de la page d’erreur lors de l’application est exécutée.

Les étapes précédentes doivent être répété lorsque des modifications sont apportées au modèle.

<a name="identity-model"></a>

## <a name="the-identity-model"></a>Le modèle d’identité

### <a name="entity-types"></a>Types d'entité

Le modèle d’identité se compose de sept types d’entités :

* `User` -représente l’utilisateur
* `Role` -représente un rôle
* `UserClaim` -représente une revendication possession par l’utilisateur
* `UserToken` -représente un jeton d’authentification pour un utilisateur
* `UserLogin` -associe un utilisateur avec un compte de connexion
* `RoleClaim` -représente une revendication qui est accordée à tous les utilisateurs au sein d’un rôle
* `UserRole` -joindre l’entité qui associe des utilisateurs et des rôles

### <a name="entity-type-relationships"></a>Relations de type d’entité

Ces types d’entités sont associées à l’autre des manières suivantes :

* Chaque `User` peut avoir de nombreux `UserClaims`
* Chaque `User` peut avoir de nombreux `UserLogins`
* Chaque `User` peut avoir de nombreux `UserTokens`
* Chaque `Role` peut avoir de nombreux associés `RoleClaims`
* Chaque `User` peut avoir de nombreux associés `Roles`et chaque `Role` peut être associé à de nombreux utilisateurs
  * Il s’agit d’une relation plusieurs-à-plusieurs, ce qui nécessite une table de jointure dans la base de données. La table de jointure est représentée par la `UserRole` entité.

### <a name="default-model-configuration"></a>Configuration de modèle par défaut

Identité définit une série de « classes de contexte » qui héritent de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) pour configurer et utiliser le modèle. Cette configuration est effectuée à l’aide de la [EF Core Code première API Fluent](/ef/core/modeling/) dans les [OnModelCreating](/dotnet/api/microsoft.entityframeworkcore.dbcontext.onmodelcreating#Microsoft_EntityFrameworkCore_DbContext_OnModelCreating_Microsoft_EntityFrameworkCore_ModelBuilder_) méthode de la classe de contexte. La configuration par défaut est la suivante :

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

### <a name="model-generic-types"></a>Modèle des types génériques

Identité définit les types CLR par défaut pour chacun des types d’entités répertoriées ci-dessus. Ces types sont toutes le préfixe « Identité » : `IdentityUser`, `IdentityRole`, `IdentityUserClaim`, `IdentityUserToken`, `IdentityUserLogin`, `IdentityRoleClaim`, et `IdentityUserRole`.

Au lieu d’utiliser directement à ces types, les types peuvent être utilisés comme classes de base pour les types de l’application. Le `DbContext` classes définies par l’identité sont génériques, telles que les différents types CLR peuvent être utilisés pour un ou plusieurs types d’entités dans le modèle. Ces types génériques permettent également le type de la clé primaire d’utilisateur à modifier.

Lors de l’utilisation d’identité avec prise en charge pour les rôles, un [IdentityDbContext](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identitydbcontext) classe doit être utilisée :

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

Il est également possible d’utiliser des identités sans rôles (uniquement des revendications), auquel cas un `IdentityUserContext` classe doit être utilisée :


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

## <a name="customizing-the-model"></a>La personnalisation du modèle

Le point de départ pour personnaliser le modèle doit dériver du type de contexte approprié ; consultez la section précédente. Ce type de contexte est généralement appelé `ApplicationDbContext` et est créé par les modèles ASP.NET Core.

Le contexte est utilisé pour configurer le modèle de deux façons :
* En fournissant des types d’entité et la clé pour les paramètres de type générique.
* En substituant `OnModelCreating` pour modifier le mappage de ces types.

Lors de la substitution `OnModelCreating`, `base.OnModelCreating` doit être appelée en premier lieu, la substitution de configuration doit ensuite être appelée. EF Core a généralement une stratégie d’un dernier-wins pour la configuration. Par exemple, si le `ToTable` pour une entité de type est appelé tout d’abord avec le nom d’une table et puis plus tard avec un autre nom de table, puis le nom de table dans le deuxième appel est celle qui est utilisée.

### <a name="using-a-custom-user-type"></a>À l’aide d’un type d’utilisateur personnalisé

Pour utiliser un type d’utilisateur personnalisé, créez le type et qu’il hérite de `IdentityUser`. Il est habituel de ce type de nom `ApplicationUser`. Ce type ont généralement des propriétés supplémentaires pas du type de base, sinon il ne serait aucune valeur dans sa création. Exemple :

```CSharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Ensuite, utilisez ce type comme argument générique pour le contexte :

```CSharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Mise à jour `ConfigureServices` pour utiliser le nouveau `ApplicationUser` classe :

```CSharp
services.AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Il n’est pas nécessaire de substituer `OnModelCreating` ici car EF Core mappera le `CustomTag` propriété par convention. Toutefois, la base de données devra être mise à jour pour obtenir un nouveau `CustomTag` colonne. Pour ce faire, ajoutez une migration, puis mettez à jour la base de données comme décrit dans [identité et EF Core Migrations](#identity-migrations).

### <a name="changing-the-key-type"></a>Modification du type de clé

Modification du type de colonne clé primaire (PK) après avoir créé la base de données pose sur de nombreux systèmes de base de données. La modification de la clé publique généralement implique et recréer la table. Par conséquent, il est recommandé que les types de clés soient spécifiées dans la migration initiale telles que les types de clés cible sont créés lors de la création de la base de données.

Si la base de données a été créé, puis `Drop-Database` (PMC) ou `dotnet ef database drop` (.NET Core CLI) peut servir à supprimer.

Une fois qu’il est confirmé que la base de données n’existe pas, supprimez la migration initiale avec `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (.NET Core CLI).

Mise à jour la `ApplicationDbContext` à une autre classe de base, en spécifiant le nouveau type de clé de `TKey`. Par exemple, pour utiliser un `Guid` clé :

```CSharp
public class ApplicationDbContext : IdentityDbContext<IdentityUser<Guid>, IdentityRole<Guid>, Guid>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

Notez que les classes génériques `IdentityUser<TKey>` et `IdentityRole<TKey>` doit également être spécifié à utiliser le nouveau type de clé. `ConfigureServices` doit être mis à jour pour utiliser l’utilisateur générique :

```CSharp
services.AddDefaultIdentity<IdentityUser<Guid>>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Si une personnalisée `ApplicationUser` est utilisé, mettre à jour qu’elle hérite `IdentityUser<TKey>`. Exemple :

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

### <a name="adding-navigation-properties"></a>Ajout de propriétés de navigation

Modification de la configuration de modèle pour les relations peut être un plus difficile que d’autres modifications apportées. Veillez à remplacer les relations existantes au lieu de créer des relations supplémentaires. En particulier, la relation modifiée doit spécifier la même propriété de clé étrangère en tant que la relation existante. Par exemple, la relation entre `Users` et `UserClaims` est par défaut spécifié en tant que :

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

La clé étrangère pour cette relation est spécifiée en tant que le `UserClaim.UserId` propriété. `HasMany` et `WithOne` sont appelés sans arguments pour créer la relation sans les propriétés de navigation.

Ajouter une propriété de navigation `ApplicationUser` qui permettra associés `UserClaims` être référencé à partir de l’utilisateur :

```CSharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Notez que la `TKey` pour `IdentityUserClaim<TKey>` est de type spécifié pour la clé primaire d’utilisateurs--dans ce cas `string` étant donné que nous utilisons les valeurs par défaut. Il s’agit de **pas** le type de clé primaire pour la `UserClaim` type d’entité.

Maintenant que la propriété de navigation existe il doit être configuré dans `OnModelCreating`:

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

Notez que la relation est configurée tel qu’il était avant, uniquement avec une propriété de navigation spécifiée dans l’appel à `HasMany`.

Les propriétés de navigation existent uniquement dans le modèle EF, pas la base de données. Étant donné que la clé étrangère de la relation n’a pas changé, ce type de modification de modèle ne requiert pas la base de données à mettre à jour. Cela peut être vérifié en ajoutant une migration après avoir effectué la modification : le `Up` et `Down` méthodes sont vides.

### <a name="adding-all-user-navigation-properties"></a>Ajout de toutes les propriétés de navigation utilisateur

À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation unidirectionnel pour toutes les relations utilisateur :

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

### <a name="adding-user-and-role-navigation-properties"></a>Ajout de l’utilisateur et le rôle des propriétés de navigation

À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation pour toutes les relations sur l’utilisateur et le rôle :

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

Remarques :

* Cet exemple inclut également le `UserRole` joindre l’entité, qui est nécessaire pour naviguer jusqu'à la relation plusieurs-à-plusieurs à partir des utilisateurs aux rôles.
* N’oubliez pas de modifier les types des propriétés de navigation pour refléter le fait que `ApplicationXxx` types sont maintenant utilisés à la place de `IdentityXxx` types.
* N’oubliez pas d’utiliser le `ApplicationXxx` dans le modèle générique `ApplicationContext` définition.

### <a name="adding-all-navigation-properties"></a>Ajout de toutes les propriétés de navigation

À l’aide de la section ci-dessus comme guide, Voici un exemple qui configure les propriétés de navigation pour toutes les relations sur tous les types d’entités :

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

### <a name="using-composite-keys"></a>À l’aide de clés composites

Les sections précédentes ont démontré la modification du type de clé utilisée dans le modèle d’identité. Modification du modèle de clé d’identité pour utiliser des clés composites n’est pas pris en charge ou n’est pas recommandée. À l’aide d’une clé composite avec l’identité impliquerait de modifier la façon dont le code de gestionnaire d’identité interagit avec le modèle, qui est une personnalisation non pris en charge et dépasse le cadre de ce document.

### <a name="changing-tablecolumn-names-and-facets"></a>Modification des noms de colonne de la table et les facettes

Pour modifier les noms des tables et des colonnes, appelez `base.OnModelCreating`, puis ajoutez la configuration pour remplacer tous les paramètres par défaut. Par exemple, pour modifier le nom de toutes les tables d’identité :

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

Notez que ces exemples utilisent les types d’identité par défaut. Si vous utilisez un type d’application, telle que `ApplicationUser` puis configurez ce type au lieu du type de valeur par défaut.

Voici un exemple qui modifie certains noms de colonne :

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

Certains types de colonnes de la base de données peuvent être configurés avec certaines _facettes_ telles que la longueur de chaîne maximale autorisée. Voici un exemple qui définit des longueurs de colonne maximale pour plusieurs propriétés de chaîne dans le modèle :

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

### <a name="mapping-to-a-different-schema"></a>Mappage vers un autre schéma

Les schémas peuvent se comporter différemment dans les fournisseurs de base de données différente, mais pour SQL Server, la valeur par défaut consiste à créer de toutes les tables dans le schéma « dbo ». Cela peut être modifié pour à la place créer les tables dans un schéma différent. Exemple :

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

### <a name="lazy-loading"></a>Chargement différé

Dans cette section prise en charge pour le chargement différé des proxys dans le modèle d’identité est ajouté. Chargement lent est utile, car elle permet les propriétés de navigation à être utilisée sans la première s’assurer qu’ils sont chargés.

Types d’entités est possible adaptées à chargement différé de plusieurs façons, comme décrit dans la [EF documentations](/ef/core/querying/related-data#lazy-loading). Par souci de simplicité, nous allons utiliser des proxys de chargement différé, ce qui nécessite :

* Installation de le [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.
* Un appel à `.UseLazyLoadingProxies()` dans `AddDbContext`.
* Types d’entités publiques avec les propriétés de navigation virtuel public.

Voici un exemple de l’appel de `.UseLazyLoadingProxies()`:

```CSharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
            .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Consultez les exemples ci-dessus pour ajouter des propriétés de navigation pour les types d’entités.
