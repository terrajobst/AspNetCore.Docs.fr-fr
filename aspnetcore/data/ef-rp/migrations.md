---
title: Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8
author: rick-anderson
description: Dans ce didacticiel, vous allez commencer à utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC.
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: ef79aca700068aadf9b5b2b689213b6e78a5d729
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346435"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a>Pages Razor avec EF Core dans ASP.NET Core - Migrations - 4 sur 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Par [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Dans ce didacticiel, nous allons utiliser la fonctionnalité de migrations EF Core pour gérer les modifications du modèle de données.

Si vous rencontrez des problèmes que vous ne pouvez pas résoudre, téléchargez [l’application terminée](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

Quand une nouvelle application est développée, le modèle de données change fréquemment. Chaque fois que le modèle change, il est désynchronisé avec la base de données. Ce didacticiel commence par configurer Entity Framework pour créer la base de données si elle n’existe pas. Chaque fois que le modèle de données change :

* La base de données est supprimée
* EF crée une nouvelle base de données qui correspond au modèle
* L’application amorce la base de données avec des données de test

Cette approche pour conserver la synchronisation de la base de données avec le modèle de données fonctionne bien jusqu’à ce que vous déployiez l’application en production. Quand l’application s’exécute en production, elle stocke généralement des données qui doivent être tenues à jour. L’application ne peut pas commencer avec une base de données de test chaque fois qu’une modification est apportée (par exemple en cas d’ajout d’une nouvelle colonne). La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF Core de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.

Plutôt que de supprimer et de recréer la base de données quand le modèle de données change, les migrations mettent à jour le schéma et conservent les données existantes.

## <a name="drop-the-database"></a>Supprimer la base de données

Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande `database drop` :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dans la **console du Gestionnaire de package**, exécutez la commande suivante :

```PMC
Drop-Database
```

Exécutez `Get-Help about_EntityFrameworkCore` à partir de la console du Gestionnaire de package pour obtenir des informations d’aide.

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Ouvrez une fenêtre de commande et accédez au dossier du projet. Le dossier du projet contient le fichier *Startup.cs*.

Entrez ce qui suit dans la fenêtre de commande :

 ```console
 dotnet ef database drop
 ```

------

## <a name="create-an-initial-migration-and-update-the-db"></a>Créer une migration initiale et mettre à jour la base de données

Générez le projet et créez la première migration.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

------

### <a name="examine-the-up-and-down-methods"></a>Examiner les méthodes Up et Down

La commande EF Core `migrations add` a généré du code pour créer la base de données. Ce code de migrations se trouve dans le fichier *Migrations\<horodatage> _InitialCreate.cs*. La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données. La méthode `Down` les supprime, comme indiqué dans l’exemple suivant :

[!code-csharp[](intro/samples/cu21/Migrations/20180626224812_InitialCreate.cs?range=7-24,77-88)]

Les migrations appellent la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour restaurer la mise à jour, les migrations appellent la méthode `Down`.

Le code précédent concerne la migration initiale. Ce code a été créé quand la commande `migrations add InitialCreate` a été exécutée. Le paramètre de nom de migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier. Le nom de la migration peut être n’importe quel nom de fichier valide. Nous vous conseillons de choisir un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, une migration ajoutant une table de département pourrait se nommer « TableAjoutDépartement ».

Si la migration initiale est créée et que la base de données existe :

* Le code de création de base de données est généré
* Le code de création de base de données n’a pas besoin de s’exécuter, car la base de données correspond déjà au modèle de données. Si le code de création de base de données est exécuté, il n’apporte aucune modification, car la base de données correspond déjà au modèle de données.

Quand l’application est déployée sur un nouvel environnement, vous devez exécuter le code de création de base de données pour créer la base de données.

Comme la base de données a été supprimée et n’existe pas, les migrations créent une autre base de données.

### <a name="the-data-model-snapshot"></a>Capture instantanée du modèle de données

Les migrations créent un *instantané* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*. Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée.

Pour supprimer une migration, utilisez la commande suivante :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Remove-Migration

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

```console
dotnet ef migrations remove
```

Pour plus d’informations, consultez [dotnet ef migrations remove](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

------

Pour supprimer les migrations, la commande supprime la migration et garantit que l’instantané est correctement réinitialisé.

### <a name="remove-ensurecreated-and-test-the-app"></a>Supprimer EnsureCreated et tester l’application

Dans les phases initiales de développement, nous avons utilisé `EnsureCreated`. Dans ce tutoriel, nous utilisons des migrations. La commande `EnsureCreated` a les limitations suivantes :

* Elle ignore les migrations et crée la base de données et le schéma
* Elle ne crée pas de table de migrations
* Elle ne peut *pas* être utilisée avec des migrations
* Elle est conçue pour effectuer des tests et un prototypage rapide, où la base de données est supprimée et recréée fréquemment.

Supprimez la ligne suivante de `DbInitializer` :

```csharp
context.Database.EnsureCreated();
```

Exécutez l’application et vérifiez que la base de données est amorcée.

### <a name="inspect-the-database"></a>Inspecter la base de données

Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données. Notez l’ajout d’une table `__EFMigrationsHistory`. La table `__EFMigrationsHistory` effectue le suivi des migrations qui ont été appliquées à la base de données. Visualisez les données dans la table `__EFMigrationsHistory` ; elle affiche une ligne pour la première migration. Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.

Exécutez l’application et vérifiez que tout fonctionne.

## <a name="applying-migrations-in-production"></a>Application de migrations en production

Nous vous recommandons de faire en sorte que les applications de production n’appellent **pas** [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) au démarrage de l’application. `Migrate` ne doit pas être appelée à partir d’une application dans la batterie de serveurs, par exemple si l’application a été déployée dans le cloud avec montée en puissance parallèle (plusieurs instances de l’application sont en cours d’exécution).

La migration de base de données doit être effectuée dans le cadre du déploiement et de manière contrôlée. Parmi les approches de migration de base de données de production, citons :

* L’utilisation de migrations pour créer des scripts SQL et l’utilisation de scripts SQL dans le déploiement
* L’exécution de `dotnet ef database update` à partir d’un environnement contrôlé

EF Core utilise la table `__MigrationsHistory` pour voir si des migrations doivent s’exécuter. Si la base de données est à jour, aucune migration n’est exécutée.

## <a name="troubleshooting"></a>Résolution des problèmes

Téléchargez [l’application terminée](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).

L’application génère l’exception suivante :

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

Solution : Exécutez `dotnet ef database update`.

### <a name="additional-resources"></a>Ressources supplémentaires

* [Version YouTube de ce tutoriel](https://www.youtube.com/watch?v=OWSUuMLKTJo)
* [CLI .NET Core](/ef/core/miscellaneous/cli/dotnet).
* [Console du Gestionnaire de package (Visual Studio)](/ef/core/miscellaneous/cli/powershell)

::: moniker-end

> [!div class="step-by-step"]
> [Précédent](xref:data/ef-rp/sort-filter-page)
> [Suivant](xref:data/ef-rp/complex-data-model)
