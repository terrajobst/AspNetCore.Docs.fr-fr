---
title: Personnalisation de modèle d’identité dans ASP.NET Core
author: ajcvickers
description: Cet article décrit comment personnaliser le modèle de données Entity Framework Core sous-jacent pour ASP.NET Core Identity.
ms.author: avickers
ms.date: 04/24/2019
uid: security/authentication/customize_identity_model
ms.openlocfilehash: 53ce77e20722f3ba3282ff4455a0b70d30e635b0
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536022"
---
# <a name="identity-model-customization-in-aspnet-core"></a>Personnalisation de modèle d’identité dans ASP.NET Core

Par [Arthur Vickers](https://github.com/ajcvickers)

ASP.NET Core Identity fournit une infrastructure pour la gestion et le stockage de comptes d’utilisateur dans les applications ASP.NET Core. Identité est ajoutée à votre projet lorsque **comptes d’utilisateur individuels** est sélectionné comme mécanisme d’authentification. Par défaut, identité utilise d’un Entity Framework (EF) modèle de données principal. Cet article décrit comment personnaliser le modèle d’identité.

## <a name="identity-and-ef-core-migrations"></a>Identité et les Migrations d’EF Core

Avant d’examiner le modèle, il est utile de comprendre le fonctionne de l’identité avec [Migrations d’EF Core](/ef/core/managing-schemas/migrations/) pour créer et mettre à jour une base de données. Au niveau supérieur, le processus est :

1. Définir ou mettre à jour un [modèle de données dans le code](/ef/core/modeling/).
1. Ajoutez une Migration pour traduire ce modèle en les modifications qui peuvent être appliquées à la base de données.
1. Vérifiez que la Migration représente correctement vos intentions.
1. Appliquez la Migration pour mettre à jour de la base de données pour être synchronisé avec le modèle.
1. Répétez les étapes 1 à 4 pour affiner le modèle et synchroniser la base de données.

Pour ajouter et appliquer des Migrations, utilisez une des approches suivantes :

* Le **Console du Gestionnaire de Package** fenêtre (PMC) si vous utilisez Visual Studio. Pour plus d’informations, consultez [les outils EF Core PMC](/ef/core/miscellaneous/cli/powershell).
* L’interface CLI .NET Core si à l’aide de la ligne de commande. Pour plus d’informations, consultez [outils de ligne de commande EF Core .NET](/ef/core/miscellaneous/cli/dotnet).
* En cliquant sur le **appliquer les Migrations** bouton sur la page d’erreur lors de l’application est exécutée.

ASP.NET Core possède un gestionnaire de page d’erreurs au moment du développement. Le gestionnaire peut appliquer des migrations quand l’application est exécutée. En général, les applications de production génèrent des scripts SQL dans les migrations et déploiement les modifications de base de données dans le cadre d’une application contrôlée et le déploiement de la base de données.

Lorsqu’une nouvelle application à l’aide d’identité est créée, les étapes 1 et 2 ci-dessus ont déjà été effectuées. Autrement dit, le modèle de données initiale existe déjà, et la migration initiale a été ajoutée au projet. La migration initiale doit toujours être appliquée à la base de données. La migration initiale peut être appliquée via une des approches suivantes :

* Exécutez `Update-Database` dans PMC.
* Exécutez `dotnet ef database update` dans un interpréteur de commandes.
* Cliquez sur le **appliquer les Migrations** bouton sur la page d’erreur lors de l’application est exécutée.

Répétez les étapes précédentes, comme les modifications sont apportées au modèle.

## <a name="the-identity-model"></a>Le modèle d’identité

### <a name="entity-types"></a>Types d'entité

Le modèle d’identité se compose des types d’entité suivants.

|Type d'entité|Description                                                  |
|-----------|-------------------------------------------------------------|
|`User`     |Représente l’utilisateur.                                         |
|`Role`     |Représente un rôle.                                           |
|`UserClaim`|Représente une revendication qui possède un utilisateur.                    |
|`UserToken`|Représente un jeton d’authentification pour un utilisateur.               |
|`UserLogin`|Associe un utilisateur à un compte de connexion.                              |
|`RoleClaim`|Représente une revendication qui est accordée à tous les utilisateurs au sein d’un rôle.|
|`UserRole` |Une entité de jointure qui associe des utilisateurs et des rôles.               |

### <a name="entity-type-relationships"></a>Relations de type d’entité

Le [types d’entité](#entity-types) sont liés entre eux comme suit :

* Chaque `User` peut avoir de nombreux `UserClaims`.
* Chaque `User` peut avoir de nombreux `UserLogins`.
* Chaque `User` peut avoir de nombreux `UserTokens`.
* Chaque `Role` peut avoir de nombreux associé `RoleClaims`.
* Chaque `User` peut avoir de nombreux associés `Roles`et chaque `Role` peut être associé à plusieurs `Users`. Il s’agit d’une relation plusieurs-à-plusieurs qui requiert une table de jointure dans la base de données. La table de jointure est représentée par le `UserRole` entité.

### <a name="default-model-configuration"></a>Configuration de modèle par défaut

Identité définit un grand nombre *classes de contexte* qui hérite de <xref:Microsoft.EntityFrameworkCore.DbContext> pour configurer et utiliser le modèle. Cette configuration est effectuée à l’aide de la [EF Core API Code First Fluent](/ef/core/modeling/) dans le <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating*> méthode de la classe de contexte. La configuration par défaut est :

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

### <a name="model-generic-types"></a>Modèle des types génériques

Identité définit la valeur par défaut [Common Language Runtime](/dotnet/standard/glossary#clr) types (CLR) pour chacun des types d’entités répertoriées ci-dessus. Ces types sont toutes le préfixe *identité*:

* `IdentityUser`
* `IdentityRole`
* `IdentityUserClaim`
* `IdentityUserToken`
* `IdentityUserLogin`
* `IdentityRoleClaim`
* `IdentityUserRole`

Au lieu d’utiliser directement ces types, les types peuvent être utilisés comme classes de base pour les types de l’application. Le `DbContext` classes définies par l’identité sont génériques, telles que les différents types CLR peuvent être utilisés pour un ou plusieurs des types d’entités dans le modèle. Ces types génériques permettent également le `User` type de données de (PK) clé primaire à modifier.

Lorsque vous utilisez des identités avec prise en charge pour les rôles, un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext> classe doit être utilisée. Exemple :

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

Il est également possible d’utiliser Identity sans rôles (uniquement des revendications), auquel cas un <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUserContext%601> classe doit être utilisée :

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

## <a name="customize-the-model"></a>Personnaliser le modèle

Le point de départ pour la personnalisation du modèle consiste à dériver à partir du type de contexte approprié. Consultez le [modéliser les types génériques](#model-generic-types) section. Ce type de contexte est habituellement appelé `ApplicationDbContext` et est créé par les modèles ASP.NET Core.

Le contexte est utilisé pour configurer le modèle de deux manières :

* Fourniture d’entité et les types de clés pour les paramètres de type générique.
* Substitution de `OnModelCreating` pour modifier le mappage de ces types.

Lors de la substitution `OnModelCreating`, `base.OnModelCreating` doit être appelée en premier ; la configuration de substitution doit ensuite être appelée. EF Core n’a généralement une stratégie last-celui-wins pour la configuration. Par exemple, si le `ToTable` méthode pour un type d’entité est appelée en premier avec le nom d’une table et puis à nouveau avec un autre nom de table, le nom de table dans le deuxième appel est utilisé ultérieurement.

### <a name="custom-user-data"></a>Données utilisateur personnalisées

<!--
set projNam=WebApp1
dotnet new webapp -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design 
dotnet aspnet-codegenerator identity  -dc ApplicationDbContext --useDefaultUI 
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
 -->

[Les données utilisateur personnalisées](xref:security/authentication/add-user-data) est pris en charge en héritant de `IdentityUser`. Il est courant de nommer ce type `ApplicationUser`:

```csharp
public class ApplicationUser : IdentityUser
{
    public string CustomTag { get; set; }
}
```

Utilisez le `ApplicationUser` type comme argument générique pour le contexte :

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

Il est inutile de substituer `OnModelCreating` dans la `ApplicationDbContext` classe. EF Core mappe le `CustomTag` propriété par convention. Toutefois, la base de données doit être mis à jour pour créer un nouveau `CustomTag` colonne. Pour créer la colonne, ajoutez une migration, puis mettez à jour la base de données comme décrit dans [identité et les Migrations d’EF Core](#identity-and-ef-core-migrations).

Mise à jour *Pages/Shared/_LoginPartial.cshtml* et remplacez `IdentityUser` avec `ApplicationUser`:

```cshtml
@using Microsoft.AspNetCore.Identity
@using WebApp1.Areas.Identity.Data
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager
```

Mise à jour *Areas/Identity/IdentityHostingStartup.cs* ou `Startup.ConfigureServices` et remplacez `IdentityUser` avec `ApplicationUser`.

```csharp
services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultUI();
```

Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor. Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>. Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`. Pour plus d'informations, voir :

* [Ajouter un modèle automatique d’identité](xref:security/authentication/scaffold-identity)
* [Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité](xref:security/authentication/add-user-data)

### <a name="change-the-primary-key-type"></a>Modifier le type de clé primaire

Une modification apportée au type de données de la colonne de clé primaire une fois que la base de données a été créée est problématique sur de nombreux systèmes de base de données. En général, la modification de la clé primaire implique de supprimer puis recréer la table. Par conséquent, les types de clés doivent être spécifiés dans la migration initiale lors de la création de la base de données.

Suivez ces étapes pour modifier le type de clé primaire :

1. Si la base de données a été créé avant la modification de la clé primaire, exécutez `Drop-Database` (PMC) ou `dotnet ef database drop` (CLI) .NET Core pour le supprimer.
2. Après avoir confirmé la suppression de la base de données, supprimez la migration initiale avec `Remove-Migration` (PMC) ou `dotnet ef migrations remove` (CLI .NET Core).
3. Mise à jour le `ApplicationDbContext` classe à dériver de <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext%603>. Spécifiez le type de clé pour `TKey`. Par exemple, pour utiliser un `Guid` type de clé :

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

    Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.IdentityRole%601> doit être spécifié pour utiliser le nouveau type de clé.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    Dans le code précédent, les classes génériques <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityUser%601> et <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityRole%601> doit être spécifié pour utiliser le nouveau type de clé.

    ::: moniker-end

    `Startup.ConfigureServices` doit être mis à jour pour utiliser l’utilisateur générique :

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

4. Si un personnalisé `ApplicationUser` classe est utilisée, mettez à jour de la classe d’hériter de `IdentityUser`. Exemple :

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Models/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    ::: moniker range=">= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationUser.cs?name=snippet_ApplicationUser&highlight=4)]

    ::: moniker-end

    Mise à jour `ApplicationDbContext` pour référencer personnalisé `ApplicationUser` classe :

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

    Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:

    ::: moniker range=">= aspnetcore-2.1"

    ```csharp
    services.AddDefaultIdentity<ApplicationUser>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultUI()
            .AddDefaultTokenProviders();
    ```

    Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.

    Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor. Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>. Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>()
            .AddDefaultTokenProviders();
    ```

    Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    ```csharp
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext, Guid>()
            .AddDefaultTokenProviders();
    ```

    Le <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> méthode accepte un `TKey` type indiquant le type de données de la clé primaire.

    ::: moniker-end

5. Si un personnalisé `ApplicationRole` classe est utilisée, mettez à jour de la classe d’hériter de `IdentityRole<TKey>`. Exemple :

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationRole.cs?name=snippet_ApplicationRole&highlight=4)]

    Mise à jour `ApplicationDbContext` pour référencer personnalisé `ApplicationRole` classe. Par exemple, la classe suivante fait référence à un personnalisé `ApplicationUser` et personnalisé `ApplicationRole`:

    ::: moniker range=">= aspnetcore-2.1"

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.1/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=13-16)]

    Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.

    Dans ASP.NET Core 2.1 ou version ultérieure, l’identité est fournie comme une bibliothèque de classes Razor. Pour plus d'informations, consultez <xref:security/authentication/scaffold-identity>. Par conséquent, le code précédent requiert un appel à <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>. Si le Générateur de modèles automatique identité a été utilisé pour ajouter des fichiers d’identité au projet, supprimez l’appel à `AddDefaultUI`.

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.0"

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/2.0/RazorPagesSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Le type de données de la clé primaire est déduit en analysant l'objet <xref:Microsoft.EntityFrameworkCore.DbContext>.

    ::: moniker-end

    ::: moniker range="<= aspnetcore-1.1"

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Data/ApplicationDbContext.cs?name=snippet_ApplicationDbContext&highlight=5-6)]

    Inscrire la classe de contexte de base de données personnalisées lors de l’ajout du service d’identité dans `Startup.ConfigureServices`:

    [!code-csharp[](customize-identity-model/samples/1.1/MvcSampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

    Le <xref:Microsoft.Extensions.DependencyInjection.IdentityEntityFrameworkBuilderExtensions.AddEntityFrameworkStores*> méthode accepte un `TKey` type indiquant le type de données de la clé primaire.

    ::: moniker-end

### <a name="add-navigation-properties"></a>Ajouter des propriétés de navigation

Modification de la configuration de modèle pour les relations permettre être plus difficile que d’autres modifications apportées. Veillez à remplacer les relations existantes plutôt que de créer de nouvelles relations supplémentaires. En particulier, la relation modifiée devez spécifier la même propriété (FK) clé étrangère en tant que la relation existante. Par exemple, la relation entre `Users` et `UserClaims` est, par défaut, spécifiée comme suit :

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

La clé étrangère pour cette relation est spécifiée comme le `UserClaim.UserId` propriété. `HasMany` et `WithOne` sont appelés sans arguments pour créer la relation sans propriétés de navigation.

Ajouter une propriété de navigation `ApplicationUser` qui permet d’associés `UserClaims` pour être référencé à partir de l’utilisateur :

```csharp
public class ApplicationUser : IdentityUser
{
    public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
}
```

Le `TKey` pour `IdentityUserClaim<TKey>` est du type spécifié pour la clé primaire d’utilisateurs. Dans ce cas, `TKey` est `string` car les valeurs par défaut sont utilisés. Il a **pas** le type de clé primaire pour la `UserClaim` type d’entité.

Maintenant que la propriété de navigation existe, il doit être configuré dans `OnModelCreating`:

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

Notez que la relation est configurée exactement telle qu’elle était avant, uniquement avec une propriété de navigation spécifiée dans l’appel à `HasMany`.

Les propriétés de navigation existent uniquement dans le modèle EF, pas la base de données. Étant donné que la clé étrangère pour la relation n’a pas changé, ce type de modification de modèle ne nécessite pas la base de données à mettre à jour. Cela peut être vérifié en ajoutant une migration après avoir effectué la modification. Le `Up` et `Down` méthodes sont vides.

### <a name="add-all-user-navigation-properties"></a>Ajouter toutes les propriétés de navigation utilisateur

À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation unidirectionnelle pour toutes les relations utilisateur :

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

### <a name="add-user-and-role-navigation-properties"></a>Ajouter des propriétés de navigation utilisateur et de rôle

À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur l’utilisateur et le rôle :

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

Remarques :

* Cet exemple inclut également le `UserRole` joindre l’entité, qui est nécessaire pour naviguer jusqu'à la relation plusieurs-à-plusieurs à partir des utilisateurs aux rôles.
* N’oubliez pas de modifier les types des propriétés de navigation pour les faire correspondre `ApplicationXxx` types sont maintenant utilisés au lieu de `IdentityXxx` types.
* N’oubliez pas d’utiliser le `ApplicationXxx` dans le modèle générique `ApplicationContext` définition.

### <a name="add-all-navigation-properties"></a>Ajouter toutes les propriétés de navigation

À l’aide de la section ci-dessus en tant que guide, l’exemple suivant configure les propriétés de navigation pour toutes les relations sur tous les types d’entités :

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

### <a name="use-composite-keys"></a>Utiliser des clés composites

Les sections précédentes a montré la modification du type de clé utilisée dans le modèle d’identité. Modification du modèle de clé d’identité pour utiliser des clés composites n’est pas pris en charge ni recommandé. À l’aide d’une clé composite avec l’identité implique de modifier la façon dont le code de gestionnaire d’identité interagit avec le modèle. Cette personnalisation est abordée dans ce document.

### <a name="change-tablecolumn-names-and-facets"></a>Changer les noms de table/colonne et les facettes

Pour modifier les noms des tables et colonnes, appelez `base.OnModelCreating`. Ensuite, ajoutez la configuration pour remplacer les valeurs par défaut. Par exemple, pour modifier le nom de toutes les tables d’identité :

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

Ces exemples utilisent les types d’identité par défaut. Si vous utilisez un type d’application telles que `ApplicationUser`, configurez ce type au lieu du type par défaut.

L’exemple suivant modifie certains noms de colonne :

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

Certains types de colonnes de la base de données peuvent être configurés avec certains *facettes* (par exemple, la valeur maximale `string` longueur autorisée). L’exemple suivant définit les longueurs maximales de colonne pour plusieurs `string` propriétés dans le modèle :

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

### <a name="map-to-a-different-schema"></a>Mapper à un schéma différent

Les schémas peuvent se comporter différemment sur les fournisseurs de base de données. Pour SQL Server, la valeur par défaut consiste à créer de toutes les tables dans le *dbo* schéma. Les tables peuvent être créés dans un schéma différent. Exemple :

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    base.OnModelCreating(modelBuilder);

    modelBuilder.HasDefaultSchema("notdbo");
}
```

::: moniker range=">= aspnetcore-2.1"

### <a name="lazy-loading"></a>Chargement différé

Dans cette section, la prise en charge pour les proxys de chargement différé dans le modèle d’identité est ajoutée. Le chargement différé est utile, car elle permet les propriétés de navigation à être utilisée sans la première s’assurer qu’ils sont chargés.

Types d’entité peuvent devenir adaptées à chargement différé de plusieurs façons, comme décrit dans la [documentation EF Core](/ef/core/querying/related-data#lazy-loading). Par souci de simplicité, utilisez des proxys de chargement différé, ce qui nécessite :

* Installation de la [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package.
* Un appel à <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies*> dans <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>.
* Types d’entité publique avec `public virtual` propriétés de navigation.

L’exemple suivant illustre l’appel `UseLazyLoadingProxies` dans `Startup.ConfigureServices`:

```csharp
services
    .AddDbContext<ApplicationDbContext>(
        b => b.UseSqlServer(connectionString)
              .UseLazyLoadingProxies())
    .AddDefaultIdentity<ApplicationUser>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

Consultez les exemples précédents pour obtenir des conseils sur l’ajout de propriétés de navigation aux types d’entité.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:security/authentication/scaffold-identity>

::: moniker-end
