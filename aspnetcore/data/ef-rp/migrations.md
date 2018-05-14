---
title: Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez commencer à utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: 690beaabeab098cf9b764730b1bf1bd04bf6b003
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Dans ce didacticiel, nous allons utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données.

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée pour cette phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Quand une nouvelle application est développée, le modèle de données change fréquemment. Chaque fois que le modèle change, il est désynchronisé avec la base de données. Ce didacticiel commence par configurer Entity Framework pour créer la base de données si elle n’existe pas. Chaque fois que le modèle de données change :

* La base de données est supprimée
* EF crée une nouvelle base de données qui correspond au modèle
* L’application amorce la base de données avec des données de test

Cette approche pour conserver la synchronisation de la base de données avec le modèle de données fonctionne bien jusqu’à ce que vous déployiez l’application en production. Quand l’application s’exécute en production, elle stocke généralement des données qui doivent être tenues à jour. L’application ne peut pas commencer avec une base de données de test chaque fois qu’une modification est apportée (par exemple en cas d’ajout d’une nouvelle colonne). La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF Core de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.

Plutôt que de supprimer et de recréer la base de données quand le modèle de données change, les migrations mettent à jour le schéma et conservent les données existantes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Packages NuGet Entity Framework Core pour les migrations

Pour travailler avec des migrations, utilisez la **console du Gestionnaire de package** ou l’interface de ligne de commande (CLI). Ces didacticiels montrent comment utiliser des commandes CLI. Vous trouverez des informations sur la console du Gestionnaire de package [à la fin de ce didacticiel](#pmc).

Les outils EF Core de l’interface CLI sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*, comme indiqué. **Remarque :** Ce package doit être installé en modifiant le fichier *.csproj*. Vous ne pouvez pas utiliser la commande `install-package` ou l’interface graphique utilisateur du Gestionnaire de package pour installer ce package. Modifiez le fichier *.csproj* en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **Modifier ContosoUniversity.csproj**.

Le balisage suivant montre le fichier *.csproj* mis à jour avec les outils CLI EF Core mis en surbrillance :

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Les numéros de version dans l’exemple précédent étaient les plus récents lors de la rédaction de ce didacticiel. Utilisez la même version pour les outils CLI EF Core qui se trouvent dans les autres packages.

## <a name="change-the-connection-string"></a>Changer la chaîne de connexion

Dans le fichier *appsettings.json*, remplacez le nom de la base de données dans la chaîne de connexion par ContosoUniversity2.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

La modification du nom de la base de données dans la chaîne de connexion fait en sorte que la première migration crée une nouvelle base de données. Une nouvelle base de données est créée car il n’en existe aucune portant ce nom. La modification de la chaîne de connexion n’est pas obligatoire pour la prise en main des migrations.

Une alternative au changement de nom de la base de données consiste à supprimer la base de données. Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande CLI `database drop` :

 ```console
 dotnet ef database drop
 ```

La section suivante explique comment exécuter des commandes CLI.

## <a name="create-an-initial-migration"></a>Créer une migration initiale

Générez le projet.

Ouvrez une fenêtre de commande et accédez au dossier du projet. Le dossier du projet contient le fichier *Startup.cs*.

Entrez ce qui suit dans la fenêtre de commande :

```console
dotnet ef migrations add InitialCreate
```

La fenêtre de commande affiche des informations semblables à ce qui suit :

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Si la migration échoue avec le message « *Impossible d’accéder au fichier ContosoUniversity.dll, car il est utilisé par un autre processus.* » s’affiche :

* Arrêtez IIS Express.

   * Quittez et redémarrez Visual Studio, ou
   * Recherchez l’icône IIS Express dans la barre d’état système de Windows.
   * Cliquez avec le bouton droit sur l’icône IIS Express, puis cliquez sur **ContosoUniversity > Arrêter le site**.

Si le message d’erreur « Échec de la build. » s’affiche, réexécutez la commande. Si vous obtenez cette erreur, laissez une note au bas de ce didacticiel.

### <a name="examine-the-up-and-down-methods"></a>Examiner les méthodes Up et Down

La commande EF Core `migrations add` a généré du code à partir duquel créer la base de données. Ce code de migrations se trouve dans le fichier *Migrations\<horodatage> _InitialCreate.cs*. La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données. La méthode `Down` les supprime, comme indiqué dans l’exemple suivant :

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour restaurer la mise à jour, les migrations appellent la méthode `Down`.

Le code précédent concerne la migration initiale. Ce code a été créé quand la commande `migrations add InitialCreate` a été exécutée. Le paramètre de nom de migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier. Le nom de la migration peut être n’importe quel nom de fichier valide. Nous vous conseillons de choisir un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, une migration ajoutant une table de département pourrait se nommer « TableAjoutDépartement ».

Si la migration initiale est créée et que la base de données existe :

* Le code de création de base de données est généré
* Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà au modèle de données. Si le code de création de base de données est exécuté, il n’apporte aucune modification, car la base de données correspond déjà au modèle de données.

Quand l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.

Précédemment, la chaîne de connexion a été changée de façon à utiliser un nouveau nom pour la base de données. Comme la base de données spécifiée n’existe pas, les migrations créent la base de données.

### <a name="the-data-model-snapshot"></a>Capture instantanée du modèle de données

Migrations crée une *capture instantanée* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*. Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.

Lors de la suppression d’une migration, utilisez la commande [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` supprime la migration et garantit que la capture instantanée est correctement réinitialisée.

Pour plus d’informations sur l’utilisation du fichier de capture instantanée, consultez [Migrations EF Core dans les environnements d’équipe](/ef/core/managing-schemas/migrations/teams).

## <a name="remove-ensurecreated"></a>Supprimer EnsureCreated

Pour les phases initiales de développement, nous avons utilisé la commande `EnsureCreated`. Dans ce didacticiel, nous utilisons des migrations. La commande `EnsureCreated` a les limitations suivantes :

* Elle ignore les migrations et crée la base de données et le schéma
* Elle ne crée pas de table de migrations
* Elle ne peut *pas* être utilisée avec des migrations
* Elle est conçue pour effectuer des tests et un prototypage rapide, où la base de données est supprimée et recréée fréquemment.

Supprimez la ligne suivante de `DbInitializer` :

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Appliquer la migration à la base de données pendant le développement

Dans la fenêtre de commande, entrez la commande suivante pour créer la base de données et les tables.

```console
dotnet ef database update
```

Remarque : Si la commande `update` retourne l’erreur « Échec de la build. » :

* Réexécutez la commande.
* Si elle échoue à nouveau, quittez Visual Studio, puis exécutez la commande `update`.
* Laissez un message au bas de la page.

La sortie de la commande est similaire à la sortie de la commande `migrations add`. Dans la commande précédente, les journaux des commandes SQL qui configurent la base de données sont affichés. La plupart des journaux sont omis dans l’exemple de sortie suivant :

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Pour réduire le niveau de détail dans les messages des journaux, changez les niveaux de journalisation dans le fichier *appsettings.Development.json*. Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).

Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données. Notez l’ajout d’une table `__EFMigrationsHistory`. La table `__EFMigrationsHistory` effectue le suivi des migrations qui ont été appliquées à la base de données. Visualisez les données dans la table `__EFMigrationsHistory` ; elle affiche une ligne pour la première migration. Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.

Exécutez l’application et vérifiez que tout fonctionne.

## <a name="applying-migrations-in-production"></a>Application de migrations en production

Nous vous recommandons de faire en sorte que les applications de production n’appellent **pas** [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application. `Migrate` ne doit pas être appelée à partir d’une application dans la batterie de serveurs, par exemple si l’application a été déployée dans le cloud avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).

La migration de base de données doit être effectuée dans le cadre du déploiement et de manière contrôlée. Parmi les approches de migration de base de données de production, citons :

* L’utilisation de migrations pour créer des scripts SQL et l’utilisation de scripts SQL dans le déploiement
* L’exécution de `dotnet ef database update` à partir d’un environnement contrôlé

EF Core utilise la table `__MigrationsHistory` pour voir si des migrations doivent s’exécuter. Si la base de données est à jour, aucune migration n’est exécutée.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interface CLI ou console du Gestionnaire de package

Les outils EF Core pour la gestion des migrations sont disponibles à partir :

* Des commandes CLI .NET Core.
* Des applets de commande PowerShell dans la fenêtre **Console du Gestionnaire de package** de Visual Studio.

Ce didacticiel montre comment utiliser l’interface CLI, mais certains développeurs préfèrent utiliser la console du Gestionnaire de package.

Les commandes EF Core pour la console du Gestionnaire de package se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Ce package est inclus dans le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Vous n’avez donc pas à l’installer.

**Important :** Il ne s’agit pas du même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*. Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.

Pour plus d’informations sur les commandes CLI, consultez [Interface CLI .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Pour plus d’informations sur les commandes de la console du Gestionnaire de package, consultez [Console du Gestionnaire de package (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Résolution des problèmes

Téléchargez [l’application terminée pour cette phase](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

L’application génère l’exception suivante :

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solution : Exécutez `dotnet ef database update`.

Si la commande `update` retourne l’erreur « Échec de la build. » :

* Réexécutez la commande.
* Laissez un message au bas de la page.

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/sort-filter-page)
> [Suivant](xref:data/ef-rp/complex-data-model)
