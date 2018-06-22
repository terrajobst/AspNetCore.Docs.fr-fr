---
title: Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core 2.0 Identity
author: isaac2004
description: Découvrez comment migrer des applications ASP.NET existantes à l’aide de l’authentification de l’appartenance ASP.NET Core 2.0 identité.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: migration/proper-to-2x/membership-to-core-identity
ms.openlocfilehash: 3ec22713997a74b587ef5d18e71a28668a5481e2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274103"
---
# <a name="migrate-from-aspnet-membership-authentication-to-aspnet-core-20-identity"></a>Migrer à partir de l’authentification de l’appartenance ASP.NET vers ASP.NET Core 2.0 Identity

De [Isaac Levin](https://isaaclevin.com)

Cet article décrit le schéma de base de données pour les applications ASP.NET à l’aide de l’authentification de l’appartenance ASP.NET Core 2.0 identité de la migration.

> [!NOTE]
> Ce document présente les étapes nécessaires pour migrer le schéma de base de données pour les applications basées sur l’appartenance ASP.NET pour le schéma de base de données utilisé pour ASP.NET Core Identity. Pour plus d’informations sur la migration à partir de l’authentification basée sur l’appartenance ASP.NET vers ASP.NET Identity, consultez [migrer une application existante à partir de l’appartenance de SQL pour ASP.NET Identity](/aspnet/identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity). Pour plus d’informations sur ASP.NET Core Identity, consultez [Introduction à l’identité sur ASP.NET Core](xref:security/authentication/identity).

## <a name="review-of-membership-schema"></a>Révision du schéma de l’appartenance

Avant d’ASP.NET 2.0, les développeurs ont été chargés de créer l’ensemble du processus d’authentification et d’autorisation pour leurs applications. Avec ASP.NET 2.0, l’appartenance a été introduite, en fournissant une solution réutilisable pour gérer la sécurité dans les applications ASP.NET. Les développeurs ont désormais en mesure d’amorçage d’un schéma dans une base de données SQL Server avec le [aspnet_regsql.exe](https://msdn.microsoft.com/library/ms229862.aspx) commande. Après avoir exécuté cette commande, les tables suivantes ont été créées dans la base de données.

  ![Tables d’appartenances](identity/_static/membership-tables.png)

Pour migrer des applications existantes vers ASP.NET Core 2.0 Identity, les données dans ces tables doivent être migrés vers les tables utilisées par le nouveau schéma d’identité.

## <a name="aspnet-core-identity-20-schema"></a>Schéma de base identité 2.0 d’ASP.NET

ASP.NET Core 2.0 suit le [identité](/aspnet/identity/index) principe introduit dans ASP.NET 4.5. Si le principe est partagé, l’implémentation entre les infrastructures est différente, même entre les versions de ASP.NET Core (consultez [migrer l’authentification et identité vers ASP.NET Core 2.0](xref:migration/1x-to-2x/index)).

Le moyen le plus rapide pour afficher le schéma pour l’identité de ASP.NET Core 2.0 doit créer une application ASP.NET Core 2.0. Suivez ces étapes dans Visual Studio 2017 :

* Sélectionnez **Fichier** > **Nouveau** > **Projet**.
* Créer un nouveau **Application ASP.NET Core Web**, puis nommez le projet *CoreIdentitySample*.
* Sélectionnez **ASP.NET Core 2.0** dans la liste déroulante, puis sélectionnez **Application web**. Ce modèle génère un [Pages Razor](xref:razor-pages/index) application. Avant de cliquer sur **OK**, cliquez sur **modifier l’authentification**.
* Choisissez **comptes d’utilisateur individuels** pour les modèles d’identité. Enfin, cliquez sur **OK**, puis **OK**. Visual Studio crée un projet à l’aide du modèle d’identité de base ASP.NET.

Identité de ASP.NET Core 2.0 utilise [Entity Framework Core](/ef/core) pour interagir avec la base de données stockant les données d’authentification. Dans l’ordre de l’application nouvellement créée fonctionner, il doit être une base de données pour stocker ces données. Après la création d’une nouvelle application, la plus rapide pour inspecter le schéma dans un environnement de base de données consiste à créer la base de données à l’aide des migrations d’Entity Framework. Ce processus crée une base de données, soit localement ou ailleurs, qui reproduit ce schéma. Passez en revue la documentation précédente pour plus d’informations.

Pour créer une base de données avec le schéma de l’identité de ASP.NET Core, exécutez le `Update-Database` dans Visual Studio **Package Manager Console** les fenêtre (PMC)&mdash;il se trouve dans **outils**  >  **Gestionnaire de Package NuGet** > **Package Manager Console**. PMC prend en charge les commandes Entity Framework en cours d’exécution.

Commandes de Entity Framework utilisent la chaîne de connexion pour la base de données spécifiée dans *appsettings.json*. La chaîne de connexion cible d’une base de données sur *localhost* nommé *asp net-core identité*. Dans ce paramètre, Entity Framework est configuré pour utiliser le `DefaultConnection` chaîne de connexion.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=aspnet-core-identity;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

Cette commande crée la base de données avec le schéma et toutes les données requises pour l’initialisation de l’application. L’image suivante illustre la structure de table qui est créée avec les étapes précédentes.

   ![Tables d’identité](identity/_static/identity-tables.png)

## <a name="migrate-the-schema"></a>migrer le schéma

Il existe de légères différences dans les champs pour l’appartenance et ASP.NET Core Identity et les structures de table. Le modèle a changé considérablement pour l’authentification/autorisation avec les applications ASP.NET et ASP.NET Core. Les objets clés sont toujours utilisés avec l’identité sont *utilisateurs* et *rôles*. Tables de mappage pour *utilisateurs*, *rôles*, et *UserRoles*.

### <a name="users"></a>Utilisateurs

| *Identity(AspNetUsers)* |   | *Membership(aspnet_Users/aspnet_Membership)* ||
| --- | --- | --- | --- | --- | --- |
| **Nom de champ** | **Type**  |   **Nom de champ** | **Type**  |
|`Id` | chaîne | `aspnet_Users.UserId` | chaîne
|`UserName` | chaîne | `aspnet_Users.UserName` | chaîne
|`Email` | chaîne | `aspnet_Membership.Email` | chaîne
|`NormalizedUserName` | chaîne | `aspnet_Users.LoweredUserName` | chaîne
|`NormalizedEmail` | chaîne | `aspnet_Membership.LoweredEmail` | chaîne
|`PhoneNumber` | chaîne | `aspnet_Users.MobileAlias` | chaîne
|`LockoutEnabled` | bit | `aspnet_Membership.IsLockedOut` | bit

> [!NOTE]
> Pas tous les mappages de champs sont semblables aux relations de l’appartenance ASP.NET Core identité. Le tableau précédent prend le schéma utilisateur d’appartenance par défaut et il mappe au schéma ASP.NET Core Identity. Tous les autres champs personnalisés qui ont été utilisés pour l’appartenance doivent être mappés manuellement. Dans ce mappage, aucun mappage n’existe pour les mots de passe comme critères de mot de passe et un mot de passe sels ne migrent pas entre les deux. **Il est recommandé de laisser le mot de passe avec la valeur null et demander aux utilisateurs de réinitialiser leurs mots de passe.** Dans ASP.NET Core Identity, `LockoutEnd` doit être définie par une date dans le futur, si l’utilisateur est verrouillé. Cela est illustré dans le script de migration.

### <a name="roles"></a>Rôles

| *Identity(AspNetRoles)* |   | *Membership(aspnet_Roles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nom de champ** | **Type**  |   **Nom de champ** | **Type**  |
|`Id` | chaîne | `RoleId` | chaîne
|`Name` | chaîne | `RoleName` | chaîne
|`NormalizedName` | chaîne | `LoweredRoleName` | chaîne

### <a name="user-roles"></a>Rôles d’utilisateur

| *Identity(AspNetUserRoles)* |   | *Membership(aspnet_UsersInRoles)* ||
| --- | --- | --- | --- | --- | --- |
| **Nom de champ** | **Type**  |   **Nom de champ** | **Type**  |
|`RoleId` | chaîne | `RoleId` | chaîne
|`UserId` | chaîne | `UserId` | chaîne

Référencez les tables de mappage précédent lors de la création d’un script de migration pour *utilisateurs* et *rôles*. L’exemple suivant suppose que vous disposez de deux bases de données sur un serveur de base de données. Une base de données contient les données et le schéma existant de l’appartenance d’ASP.NET. La base de données a été créé à l’aide des étapes décrites précédemment. Les commentaires sont incluses inline pour plus de détails.

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
       --Security Stamp is a token used to verify the state of an account and is subject to change at any time. It should be intialized as a new ID.
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

Après l’achèvement de ce script, l’application ASP.NET Core identité créée précédemment est remplie avec les utilisateurs d’appartenance. Les utilisateurs doivent modifier leur mot de passe avant de vous connecter.

> [!NOTE]
> Si le système d’appartenance avait des utilisateurs avec des noms d’utilisateur qui ne correspondaient pas leur adresse de messagerie, les modifications sont requises pour l’application créée précédemment pour ce faire. Le modèle par défaut attend `UserName` et `Email` identiques. Dans les situations dans lesquelles ils sont différents, le processus de connexion doit être modifiée pour utiliser `UserName` au lieu de `Email`.

Dans le `PageModel` de la Page de connexion, situé dans *Pages\Account\Login.cshtml.cs*, supprimer le `[EmailAddress]` attribut à partir de la *par courrier électronique* propriété. Renommez-le *nom d’utilisateur*. Cela nécessite un changement dans la mesure `EmailAddress` est mentionné dans le *vue* et *PageModel*. Le résultat ressemble à ceci :

 ![Connexion fixe](identity/_static/fixed-login.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris comment les utilisateurs d’appartenance SQL à ASP.NET Core 2.0 identité du port. Pour plus d’informations sur ASP.NET Core Identity, consultez [Introduction à identité](xref:security/authentication/identity).
