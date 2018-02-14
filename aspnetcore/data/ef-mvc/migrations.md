---
title: ASP.NET Core MVC avec EF Core - Migrations - 4 sur 10
author: tdykstra
description: "Dans ce didacticiel, vous utilisez la fonctionnalité Migrations d’EF Core pour gérer les modifications du modèle de données dans une application ASP.NET Core MVC."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/migrations
ms.openlocfilehash: fd466af8a73bf4c568fafe7e7fdcaa82021624da
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a>Migrations - Didacticiel EF Core avec ASP.NET Core MVC (4 sur 10)

Par [Tom Dykstra](https://github.com/tdykstra) et [Rick Anderson](https://twitter.com/RickAndMSFT)

L’exemple d’application web Contoso University montre comment créer des applications web ASP.NET Core MVC avec Entity Framework Core et Visual Studio. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](intro.md).

Dans ce didacticiel, vous utilisez la fonctionnalité Migrations d’EF Core pour gérer les modifications du modèle de données. Dans les didacticiels suivants, vous allez ajouter d’autres migrations au fil de la modification du modèle de données.

## <a name="introduction-to-migrations"></a>Introduction aux migrations

Quand vous développez une nouvelle application, votre modèle de données change fréquemment et, chaque fois que le modèle change, il n’est plus en synchronisation avec la base de données. Vous avez démarré ce didacticiel en configurant Entity Framework pour créer la base de données si elle n’existait pas. Ensuite, chaque fois que vous modifiez le modèle de données, en ajoutant, supprimant ou changeant des classes d’entité ou votre classe DbContext, vous pouvez supprimer la base de données : EF en crée alors une nouvelle qui correspond au modèle et l’alimente avec des données de test.

Cette méthode pour conserver la base de données en synchronisation avec le modèle de données fonctionne bien jusqu’au déploiement de l’application en production. Quand l’application s’exécute en production, elle stocke généralement les données que vous voulez conserver, et vous ne voulez pas tout perdre chaque fois que vous apportez une modification, comme ajouter une nouvelle colonne. La fonctionnalité Migrations d’EF Core résout ce problème en permettant à EF de mettre à jour le schéma de base de données au lieu de créer une nouvelle base de données.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Packages NuGet Entity Framework Core pour les migrations

Pour effectuer des migrations, vous pouvez utiliser la **console du Gestionnaire de package** ou l’interface de ligne de commande (CLI).  Ces didacticiels montrent comment utiliser des commandes CLI. Vous trouverez des informations sur la console du Gestionnaire de package [à la fin de ce didacticiel](#pmc).

Les outils EF de l’interface de ligne de commande (CLI) sont fournis dans [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Pour installer ce package, ajoutez-le à la collection `DotNetCliToolReference` dans le fichier *.csproj*, comme indiqué. **Remarque :** Vous devez installer ce package en modifiant le fichier *.csproj*. Vous ne pouvez pas utiliser la commande `install-package` ni l’interface graphique utilisateur du Gestionnaire de package. Vous pouvez modifier le fichier *.csproj* en cliquant sur le nom du projet dans **l’Explorateur de solutions** et en sélectionnant **Modifier ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Les numéros de version de cet exemple étaient les plus récents lors de la rédaction de ce didacticiel.) 

## <a name="change-the-connection-string"></a>Changer la chaîne de connexion

Dans le fichier *appsettings.json*, remplacez le nom de la base de données dans la chaîne de connexion par ContosoUniversity2 ou par un autre nom que vous n’avez pas utilisé sur l’ordinateur que vous utilisez.

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

Cette modification configure le projet de façon à ce que la première migration crée une nouvelle base de données. Ce n’est pas obligatoire quand vous commencez à utiliser les migrations, mais vous verrez plus tard pourquoi c’est judicieux.

> [!NOTE]
> Au lieu de changer le nom de la base de données, vous pouvez la supprimer. Utilisez **l’Explorateur d’objets SQL Server** (SSOX) ou la commande CLI `database drop` :
> ```console
> dotnet ef database drop
> ```
> La section suivante explique comment exécuter des commandes CLI.

## <a name="create-an-initial-migration"></a>Créer une migration initiale

Enregistrez vos modifications et générez le projet. Ouvrez ensuite une fenêtre Commande et accédez au dossier du projet. Voici un moyen rapide pour le faire :

* Dans **l’Explorateur de solutions**, cliquez sur le projet et choisissez **Ouvrir dans l’Explorateur de fichiers** dans le menu contextuel.

  ![Élément de menu Ouvrir dans l’Explorateur de fichiers](migrations/_static/open-in-file-explorer.png)

* Entrez « cmd » dans la barre d’adresses et appuyez sur Entrée.

  ![Ouvrir une fenêtre Commande](migrations/_static/open-command-window.png)

Entrez la commande suivante dans la fenêtre Commande :

```console
dotnet ef migrations add InitialCreate
```

Vous voyez une sortie similaire à celle-ci dans la fenêtre Commande :

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Si vous voyez un message d’erreur *Aucun exécutable trouvé correspondant à la commande « dotnet-ef »*, consultez [ce billet de blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) pour résoudre ce problème.

Si vous voyez un message d’erreur « *Impossible d’accéder au fichier... ContosoUniversity.dll, car il est utilisé par un autre processus.* », recherchez l’icône IIS Express dans la barre d’état système de Windows, cliquez avec le bouton droit, puis cliquez sur **ContosoUniversity > Arrêter le Site**.

## <a name="examine-the-up-and-down-methods"></a>Examiner les méthodes Up et Down

Quand vous avez exécuté la commande `migrations add`, EF a généré le code qui crée la base de données à partir de zéro. Ce code se trouve dans le dossier *Migrations*, dans le fichier nommé *\<horodatage>_InitialCreate.cs*. La méthode `Up` de la classe `InitialCreate` crée les tables de base de données qui correspondent aux jeux d’entités du modèle de données, et la méthode `Down` les supprime, comme indiqué dans l’exemple suivant.

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

La fonctionnalité Migrations appelle la méthode `Up` pour implémenter les modifications du modèle de données pour une migration. Quand vous entrez une commande pour annuler la mise à jour, Migrations appelle la méthode `Down`.

Ce code est celui de la migration initiale qui a été créé quand vous avez entré la commande `migrations add InitialCreate`. Le paramètre de nom de la migration (« InitialCreate » dans l’exemple) est utilisé comme nom de fichier ; vous pouvez le choisir librement. Nous vous conseillons néanmoins de choisir un mot ou une expression qui résume ce qui est effectué dans la migration. Par exemple, vous pouvez nommer une migration ultérieure « AjouterTableDépartement ».

Si vous avez créé la migration initiale alors que la base de données existait déjà, le code de création de la base de données est généré, mais il n’est pas nécessaire de l’exécuter, car la base de données correspond déjà au modèle de données. Quand vous déployez l’application sur un autre environnement où la base de données n’existe pas encore, ce code est exécuté pour créer votre base de données : il est donc judicieux de le tester au préalable. C’est la raison pour laquelle vous avez précédemment changé le nom de la base de données dans la chaîne de connexion : les migrations doivent pouvoir créer une base de données à partir de zéro.

## <a name="examine-the-data-model-snapshot"></a>Examiner la capture instantanée du modèle de données

Migrations crée aussi une *capture instantanée* du schéma de base de données actuel dans *Migrations/SchoolContextModelSnapshot.cs*. Voici à quoi ressemble ce code :

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

Le schéma de base de données actuel étant représenté dans le code, EF Core n’a pas à interagir avec la base de données pour créer des migrations. Quand vous ajoutez une migration, EF détermine ce qui a changé en comparant le modèle de données au fichier de capture instantanée. EF interagit avec la base de données seulement quand il doit la mettre à jour. 

Le fichier de capture instantanée doit rester synchronisé avec les migrations qui le créent : , vous ne pouvez donc pas supprimer une migration simplement en supprimant le fichier nommé *\<horodatage>_\<nom_migration>.cs*. Si vous supprimez ce fichier, les migrations restantes ne sont plus synchronisées avec le fichier de capture instantanée de la base de données. Pour supprimer la dernière migration que vous avez ajoutée, utilisez la commande [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove).

## <a name="apply-the-migration-to-the-database"></a>Appliquer la migration à la base de données

Dans la fenêtre Commande, entrez la commande suivante pour créer la base de données et ses tables.

```console
dotnet ef database update
```

La sortie de la commande est similaire à la commande `migrations add`, à ceci près que vous voyez des journaux pour les commandes SQL qui configurent la base de données. La plupart des journaux sont omis dans l’exemple de sortie suivant. Si vous préférez ne pas voir ce niveau de détail dans les messages des journaux, vous pouvez changer le niveau de journalisation dans le fichier *appsettings.Development.json*. Pour plus d’informations, consultez [Introduction à la journalisation](xref:fundamentals/logging/index).

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

Utilisez **l’Explorateur d’objets SQL Server** pour inspecter la base de données, comme vous l’avez fait dans le premier didacticiel.  Vous pouvez noter l’ajout d’une table __EFMigrationsHistory, qui fait le suivi des migrations qui ont été appliquées à la base de données. Visualisez les données de cette table : vous y voyez une ligne pour la première migration. (Le dernier journal dans l’exemple de sortie CLI précédent montre l’instruction INSERT qui crée cette ligne.)

Exécutez l’application pour vérifier que tout fonctionne toujours comme avant.

![Page Index des étudiants](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>Interface CLI ou console du Gestionnaire de package

Les outils EF pour la gestion des migrations sont disponibles à partir de commandes CLI .NET Core ou d’applets de commande PowerShell dans la fenêtre **Console du Gestionnaire de package** de Visual Studio. Ce didacticiel montre comment utiliser l’interface CLI, mais vous pouvez utiliser la console du Gestionnaire de package si vous préférez.

Les commandes EF pour la console du Gestionnaire de package se trouvent dans le package [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Ce package est déjà inclus dans le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Vous n’avez donc pas à l’installer.

**Important :** Il ne s’agit pas du même package que celui que vous installez pour l’interface CLI en modifiant le fichier *.csproj*. Le nom de celui-ci se termine par `Tools`, contrairement au nom du package CLI qui se termine par `Tools.DotNet`.

Pour plus d’informations sur les commandes CLI, consultez [Interface CLI .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet). 

Pour plus d’informations sur les commandes de la console du Gestionnaire de package, consultez [Console du Gestionnaire de package (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment créer et appliquer votre première migration. Dans le didacticiel suivant, vous allez aborder des sujets plus avancés en développant le modèle de données. Au cour de ce processus, vous allez créer et appliquer d’autres migrations.

>[!div class="step-by-step"]
[Précédent](sort-filter-page.md)
[Suivant](complex-data-model.md)  
