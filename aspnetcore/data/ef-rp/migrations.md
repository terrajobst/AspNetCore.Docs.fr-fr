---
title: Pages Razor avec EF Core - Migrations - 4 de 8
author: rick-anderson
description: Dans ce didacticiel, vous démarrez à l’aide de la fonctionnalité de migrations EF principales pour la gestion des modifications du modèle de données dans une application ASP.NET MVC de base.
manager: wpickett
ms.author: riande
ms.date: 10/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/migrations
ms.openlocfilehash: e89d95702cb94556bc6e5dc73253c51acaa11578
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="migrations---ef-core-with-razor-pages-tutorial-4-of-8"></a>Migrations - EF Core avec le didacticiel sur les Pages Razor (4 sur 8)

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Dans ce didacticiel, la fonctionnalité de migrations EF Core pour la gestion des modifications du modèle de données est utilisée.

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez le [application terminée pour cette étape](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

Lorsqu’une application est développée, le modèle de données change fréquemment. Chaque fois que des modifications sont apportées au modèle, le modèle est désynchronisé avec la base de données. Ce didacticiel a démarré en configurant Entity Framework pour créer la base de données si elle n’existe pas. Chaque fois que le modèle de données change :

* La base de données est supprimée.
* EF en crée une nouvelle qui correspond au modèle.
* L’application est basée sur la base de données avec des données de test.

Cette approche pour conserver la base de données synchronisée avec le modèle de données fonctionne bien jusqu'à ce que vous déployez l’application en production. Lorsque l’application est en cours d’exécution en production, il est généralement stocker des données qui doivent être géré. L’application ne peut pas commencer par un test de base de données chaque fois qu’une modification est effectuée (par exemple, l’ajout d’une nouvelle colonne). La fonctionnalité EF Core Migrations résout ce problème en activant Core EF mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.

Au lieu de supprimer et recréer la base de données lors de la modification du modèle de données, les migrations mettent à jour le schéma et conservent les données existantes.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Packages NuGet Entity Framework Core pour les migrations

Pour utiliser des migrations, utilisez le **Package Manager Console** (PMC) ou de l’interface de ligne de commande (CLI). Ces didacticiels montrent comment utiliser les commandes CLI. Plus d’informations sur la CFP est à [la fin de ce didacticiel](#pmc).

Les outils EF principaux pour l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Pour installer ce package, ajoutez-la à la `DotNetCliToolReference` collection dans le *.csproj* de fichiers, comme indiqué. **Remarque :** ce package doit être installé en modifiant le *.csproj* fichier. Le`install-package` commande ou l’interface utilisateur graphique du Gestionnaire de package ne peut pas être utilisé pour installer ce package. Modifier la *.csproj* fichier en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **ContosoUniversity.csproj de modifier**.

Le balisage suivant illustre la mise à jour du fichier *.csproj* avec les outils EF Core CLI mis en surbrillance :

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
Les numéros de version dans l’exemple précédent étaient en cours lorsque le didacticiel a été écrit. Utilisez la même version pour les outils EF Core CLI trouvés dans les autres packages.

## <a name="change-the-connection-string"></a>Modifier la chaîne de connexion

Dans le *appsettings.json* , modifiez le nom de la base de données dans la chaîne de connexion à ContosoUniversity2.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

La modification du nom de la base de données dans la chaîne de connexion entraîne la première migration pour créer une nouvelle base de données. Une nouvelle base de données est créée, car aucune portant ce nom n’existe pas. La modification de la chaîne de connexion n’est pas requise pour la prise en main des migrations.

Une alternative à la modification du nom de la base de données est de supprimer la base de données. Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande CLI `database drop`:

 ```console
 dotnet ef database drop
 ```

La section suivante explique comment exécuter des commandes CLI.

## <a name="create-an-initial-migration"></a>Créer une migration initiale

Générez le projet.

Ouvrez une fenêtre de commande et accédez au dossier du projet. Le dossier du projet contient le fichier *Startup.cs*.

Dans la fenêtre de commande, entrez ce qui suit :

```console
dotnet ef migrations add InitialCreate
```

La fenêtre de commande affiche des informations similaires à ce qui suit :

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

Si la migration échoue avec le message «*Impossible d’accéder au fichier... ContosoUniversity.dll, car il est utilisé par un autre processus.*  " s’affiche : s’affiche :

* Arrêtez IIS Express.

   * Quittez et redémarrez Visual Studio, ou
   * Recherchez l’icône IIS Express dans la barre d’état du système Windows.
   * Cliquez avec le bouton droit sur l’icône IIS Express, puis cliquez sur **ContosoUniversity > Arrêter le site**.

Si le message d’erreur « Échec de la génération. » s’affiche, exécutez à nouveau la commande. Si vous obtenez cette erreur, laissez une note au bas de ce didacticiel.

### <a name="examine-the-up-and-down-methods"></a>Examiner le haut et vers le bas de méthodes

La commande EF Core `migrations add` génère le code pour créer la base de données. Ce code de migrations se trouve dans le fichier *Migrations\<timestamp > _InitialCreate.cs*. La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités de modèle données. La méthode `Down` les supprime, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

Les migrations appellent la méthode `Up` pour implémenter les modifications de modèle de données pour une migration. Lorsque vous entrez une commande pour annuler la mise à jour, les migrations appellent la méthode `Down`.

Le code précédent est pour la migration initiale. Ce code a été créé lorsque le `migrations add InitialCreate` commande a été exécutée. Le paramètre de nom de la migration (« InitialCreate » dans l’exemple) est utilisé pour le nom de fichier. Le nom de la migration peut être n’importe quel nom de fichier valide. Il est conseillé de choisir un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, une migration à l’ajout d’un tableau de service peut être appelée « AddDepartmentTable ».

Si la migration initiale est créée et la base de données s’arrête :

* Le code de création de base de données est généré.
* Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà au modèle de données. Si le code de création de la base de données est exécuté, il n’apporte aucune modification car la base de données correspond déjà au modèle de données.

Lorsque l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.

Précédemment, la chaîne de connexion a été modifiée pour utiliser un nouveau nom pour la base de données.  La base de données spécifiée n’existe pas, donc les migrations créent la base de données.

### <a name="examine-the-data-model-snapshot"></a>Examiner l’instantané du modèle de données

Les migrations créent un *instantané* du schéma de base de données en cours dans *Migrations/SchoolContextModelSnapshot.cs*:

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Étant donné que le schéma de base de données actuels est représenté dans le code, EF Core ne doit pas interagir avec la base de données pour créer des migrations. Lorsque vous ajoutez une migration, EF Core détermine ce qui a changé en comparant le modèle de données pour le fichier d’instantané. EF Core interagit avec la base de données uniquement lorsqu’il a mettre à jour la base de données.

Le fichier d’instantané doit être synchronisé avec les migrations qui l’a créée. Une migration ne peut pas être supprimée en supprimant le fichier nommé  *\<timestamp > _\<migrationname > .cs*. Si ce fichier est supprimé, les migrations restantes sont synchronisées avec le fichier de capture instantanée de base de données. Pour supprimer la dernière migration ajoutée, utilisez la [supprimer des migrations d’ef dotnet](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) commande.

## <a name="remove-ensurecreated"></a>Supprimer EnsureCreated

Pour le développement initial, la commande `EnsureCreated` a été utilisée. Dans ce didacticiel, les migrations sont utilisées. `EnsureCreated` présente les limitations suivantes :

* Ignore les migrations et crée la base de données et le schéma.
* Ne crée pas une table de migration.
* Ne peut *pas* être utilisé avec des migrations.
* Est conçu pour des tests ou pour un prototypage rapide dans lequel la base de données est supprimée et recréée fréquemment.

Supprimez la ligne suivante à partir de `DbInitializer`:

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a>Appliquer la migration vers la base de données en cours de développement

Dans la fenêtre de commande, entrez la commande suivante pour créer la base de données et des tables.

```console
dotnet ef database update
```

Remarque : Si la commande `update` renvoie l’erreur « Échec de la Build. » :

* Réexécutez la commande.
* Si elle échoue de nouveau, quittez Visual Studio, puis exécutez la commande `update`.
* Laisser un message au bas de la page.

La sortie de la commande est similaire à la sortie de la commande `migrations add`. Dans la commande précédente, les journaux pour les commandes SQL qui configurent la base de données sont affichés. La plupart des journaux sont omis dans la sortie suivante :

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

Pour réduire le niveau de détail dans les messages de journal, vous pouvez modifier les niveaux de journal dans le fichier *appsettings. Development.JSON*. Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).

Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données. Notez l’ajout d’une table `__EFMigrationsHistory`. La table `__EFMigrationsHistory` effectue le suivi des migrations qui ont été appliquées à la base de données. Consultez les données de la table `__EFMigrationsHistory`: elle affiche une ligne pour la première migration. Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.

Exécutez l’application et vérifier que tout fonctionne.

## <a name="appling-migrations-in-production"></a>Migrations d’application en production

Nous vous recommandons d’applications de production doivent **pas** appeler [Database.Migrate](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application. `Migrate`ne doit pas être appelée à partir d’une application dans la batterie de serveurs. Par exemple, si l’application a été cloud déployé avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).

La migration de base de données doit être effectuée dans le cadre du déploiement et d’une façon contrôlée. Les méthodes de migration de base de données de production sont les suivantes :

* L’utilisation de migrations pour créer des scripts SQL et à l’aide de scripts SQL dans le déploiement.
* Exécuter `dotnet ef database update` à partir d’un environnement contrôlé.

EF Core utilise la table  `__MigrationsHistory` pour voir si les migrations doivent s’exécuter. Si la base de données est à jour, aucune migration n’est exécutée.

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Par rapport à l’interface de ligne de commande (CLI) Console du Gestionnaire de package (PMC)

Les outils EF Core de gestion des migrations sont disponibles à partir de :

* Commandes CLI de base .NET.
* Les applets de commande PowerShell dans la fenêtre **Console du gestionnaire de package**  de Visual Studio.

Ce didacticiel montre comment utiliser l’interface CLI, certains développeurs préfèrent à l’aide de la PMC.

Les commandes de EF Core pour PMC se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Ce package est inclus dans le metapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage), sans que vous ayez à l'installer.

**Important :** Ce n’est pas le même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*. Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.

Pour plus d’informations sur les commandes CLI, consultez [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Pour plus d’informations sur les commandes PMC, consultez [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="troubleshooting"></a>Résolution des problèmes

Téléchargez l'[application terminée pour cette étape](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

L’application génère l’exception suivante :

```text
`SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solution : exécutez `dotnet ef database update`

Si la commande `update` renvoie l’erreur « Échec de la Build. » :

* Réexécutez la commande.
* Laisser un message au bas de la page.

>[!div class="step-by-step"]
[Précédent](xref:data/ef-rp/sort-filter-page)
[Suivant](xref:data/ef-rp/complex-data-model)
