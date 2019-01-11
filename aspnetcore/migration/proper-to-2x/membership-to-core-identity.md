---
title: Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0
author: isaac2004
description: Découvrez comment migrer des applications ASP.NET existantes à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2019
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 0b7001a311eeaaa78e3d52e2ec66d33ad057c381
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207406"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core Identity 2.0

De [Isaac Levin](https://isaaclevin.com)

Cet article illustre la migration du schéma de base de données pour les applications ASP.NET à l’aide de l’authentification de l’appartenance à ASP.NET Core 2.0 Identity.

> [!NOTE]
> Ce document fournit les étapes nécessaires pour migrer le schéma de base de données pour les applications basées sur l’appartenance ASP.NET vers le schéma de base de données utilisé pour ASP.NET Core Identity. Pour plus d’informations sur la migration à partir de l’authentification basée sur l’appartenance ASP.NET vers ASP.NET Identity, consultez [migrer une application existante à partir de l’appartenance SQL vers ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity dans ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Révision de schéma d’appartenance

Avant ASP.NET 2.0, les développeurs ont été chargés de créer l’ensemble du processus d’authentification et d’autorisation pour leurs applications. Avec ASP.NET 2.0, l’appartenance a été introduite, fournissant une solution réutilisable pour la gestion de la sécurité dans les applications ASP.NET. Les développeurs ont désormais en mesure d’amorçage d’un schéma dans une base de données SQL Server avec le [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) commande. Après avoir exécuté cette commande, les tables suivantes ont été créées dans la base de données.

  ![Tables d’appartenances](identity/_static/membership-tables.png)

Pour migrer des applications existantes vers ASP.NET Core 2.0 Identity, les données dans ces tables doivent être migrés vers les tables utilisées par le nouveau schéma d’identité.

## <a name="aspnet-core-identity-20-schema"></a>ASP.NET Core Identity 2.0 schéma

ASP.NET Core 2.0 suit le [identité](/aspnet/identity/index) principe introduit dans ASP.NET 4.5. Bien que le principe est partagé, l’implémentation entre les infrastructures est différente, même entre les versions d’ASP.NET Core (consultez [migrer d’authentification et identité vers ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Le moyen le plus rapide pour afficher le schéma pour ASP.NET Core 2.0 Identity consiste à créer une application ASP.NET Core 2.0. Suivez ces étapes dans Visual Studio 2017 :

1. Sélectionnez **Fichier** > **Nouveau** > **Projet**.
1. Créer un nouveau **Application Web ASP.NET Core** projet nommé *CoreIdentitySample*.
1. Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Web Application**. Ce modèle génère un [Pages Razor](xref:razor-pages/index) application. Avant de cliquer sur **OK**, cliquez sur **modifier l’authentification**.
1. Choisissez **comptes d’utilisateur individuels** pour les modèles d’identité. Enfin, cliquez sur **OK**, puis **OK**. Visual Studio crée un projet à l’aide du modèle ASP.NET Core Identity.
1. Sélectionnez **outils** > **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package** pour ouvrir le **Console du Gestionnaire de Package** Fenêtre (PMC).
1. Accédez à la racine du projet dans PMC et exécutez le [Entity Framework (EF) Core](/ef/core) `Update-Database` commande.

    Identité ASP.NET Core 2.0 utilise EF Core pour interagir avec la base de données stockant les données d’authentification. Afin que l’application nouvellement créée fonctionne, il doit être une base de données pour stocker ces données. Après avoir créé une nouvelle application, le moyen le plus rapide pour inspecter le schéma dans un environnement de base de données consiste à créer la base de données à l’aide [Migrations d’EF Core](/ef/core/managing-schemas/migrations/). Ce processus crée une base de données, en local ou un autre emplacement, qui reproduit ce schéma. Passez en revue la documentation précédente pour plus d’informations.

    Commandes EF Core utilisent la chaîne de connexion pour la base de données spécifiée dans *appsettings.json*. La chaîne de connexion cible d’une base de données sur *localhost* nommé *asp-net-core-identity*. Dans ce paramètre, EF Core est configuré pour utiliser le `DefaultConnection` chaîne de connexion.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
      }
    }
    ```
1. Sélectionnez **vue** > **Explorateur d’objets SQL Server**. Développez le nœud correspondant au nom de la base de données spécifié dans le `ConnectionStrings:DefaultConnection` propriété du *appsettings.json*.

    Le `Update-Database` commande a créé la base de données spécifié avec le schéma et toutes les données requises pour l’initialisation d’application. L’image suivante illustre la structure de table est créée avec les étapes précédentes.

    ![Tables d’identité](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>Migrer le schéma

Il existe des différences subtiles dans les structures de table et les champs pour l’appartenance et ASP.NET Core Identity. Le modèle a des modifications substantielles pour l’authentification/autorisation avec les applications ASP.NET et ASP.NET Core. Les objets clés sont toujours utilisés avec l’identité sont *utilisateurs* et *rôles*. Voici les tables de mappage pour *utilisateurs*, *rôles*, et *UserRoles*.

### <a name="users"></a>Utilisateurs

|*Identité<br>(dbo. AspNetUsers)*        ||*L’appartenance<br>(dbo.aspnet_Users / dbo.aspnet_Membership)*||
|----------------------------------------|-----------------------------------------------------------|
|**Nom de champ**                 |**Type**|**Nom de champ**                                    |**Type**|
|`Id`                           |chaîne  |`aspnet_Users.UserId`                             |chaîne  |
|`UserName`                     |chaîne  |`aspnet_Users.UserName`                           |chaîne  |
|`Email`                        |chaîne  |`aspnet_Membership.Email`                         |chaîne  |
|`NormalizedUserName`           |chaîne  |`aspnet_Users.LoweredUserName`                    |chaîne  |
|`NormalizedEmail`              |chaîne  |`aspnet_Membership.LoweredEmail`                  |chaîne  |
|`PhoneNumber`                  |chaîne  |`aspnet_Users.MobileAlias`                        |chaîne  |
|`LockoutEnabled`               |bit     |`aspnet_Membership.IsLockedOut`                   |bit     |

> [!NOTE]
> Pas tous les mappages de champs ressemblent à des relations un à un à partir de l’appartenance à ASP.NET Core Identity. Le tableau précédent prend le schéma utilisateur d’appartenance par défaut et le mappe au schéma ASP.NET Core Identity. Tous les autres champs personnalisés qui ont été utilisés pour l’appartenance doivent être mappées manuellement. Dans ce mappage, il n’existe aucun mappage pour les mots de passe, comme critères de mot de passe et le mot de passe sels ne migrent entre les deux. **Il est recommandé de laisser le mot de passe avec la valeur null et demander aux utilisateurs de réinitialiser leurs mots de passe.** Dans ASP.NET Core Identity, `LockoutEnd` doit être définie par une date à l’avenir, si l’utilisateur est verrouillé. Ceci est illustré dans le script de migration.

### <a name="roles"></a>Rôles

|*Identité<br>(dbo. AspNetRoles)*        ||*L’appartenance<br>(dbo.aspnet_Roles)*||
|----------------------------------------|-----------------------------------|
|**Nom de champ**                 |**Type**|**Nom de champ**   |**Type**         |
|`Id`                           |chaîne  |`RoleId`         | chaîne          |
|`Name`                         |chaîne  |`RoleName`       | chaîne          |
|`NormalizedName`               |chaîne  |`LoweredRoleName`| chaîne          |

### <a name="user-roles"></a>Rôles d’utilisateur

|*Identité<br>(dbo. AspNetUserRoles)*||*L’appartenance<br>(dbo.aspnet_UsersInRoles)*||
|------------------------------------|------------------------------------------|
|**Nom de champ**           |**Type**  |**Nom de champ**|**Type**                   |
|`RoleId`                 |chaîne    |`RoleId`      |chaîne                     |
|`UserId`                 |chaîne    |`UserId`      |chaîne                     |

Référencez les tables de mappage précédent lors de la création d’un script de migration pour *utilisateurs* et *rôles*. L’exemple suivant suppose que vous avez deux bases de données sur un serveur de base de données. Une base de données contient les données et le schéma de l’appartenance ASP.NET existant. L’autre *CoreIdentitySample* base de données a été créée à l’aide des étapes décrites précédemment. Les commentaires sont incluses inline pour plus d’informations.

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

Après l’achèvement du script précédent, l’application ASP.NET Core Identity créée précédemment est renseignée avec les utilisateurs d’appartenance. Les utilisateurs doivent modifier leurs mots de passe avant de vous connecter.

> [!NOTE]
> Si le système d’appartenance avait des utilisateurs avec des noms d’utilisateur qui ne correspond pas à leur adresse de messagerie, les modifications sont requises pour l’application créée précédemment pour tenir compte de cela. Le modèle par défaut attend `UserName` et `Email` soient les mêmes. Dans les situations dans lesquelles ils sont différents, le processus de connexion doit être modifiée pour utiliser `UserName` au lieu de `Email`.

Dans le `PageModel` de la Page de connexion, situé dans *Pages\Account\Login.cshtml.cs*, supprimer le `[EmailAddress]` attribut à partir de la *E-mail* propriété. Renommez-le *nom d’utilisateur*. Cela nécessite une modification dans la mesure `EmailAddress` est mentionné dans le *vue* et *PageModel*. Le résultat ressemble à ceci :

 ![Connexion fixe](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris comment porter des utilisateurs de l’appartenance SQL vers ASP.NET Core 2.0 Identity. Pour plus d’informations sur ASP.NET Core Identity, consultez [présentation d’Identity](xref:security/authentication/identity).
