---
title: Stockage sécurisé des secrets d’application en cours de développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles comme les secrets de l’application pendant le développement d’une application ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Stockage sécurisé des secrets d’application en cours de développement dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Ce document explique les techniques pour stocker et récupérer des données sensibles pendant le développement d’une application ASP.NET Core. Vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et ne doivent pas utiliser les secrets de fabrication dans le développement ou le mode. Vous pouvez stocker et protéger des secrets de test et de production Azure avec la [fournisseur de configuration d’Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables d’environnement

Variables d’environnement sont utilisés pour éviter le stockage de secrets d’application dans le code ou dans les fichiers de configuration local. Variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifié précédemment.

::: moniker range="<= aspnetcore-1.1"
Configurer la lecture des valeurs de variable d’environnement en appelant [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) dans le `Startup` constructeur :

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Imaginez une application de web ASP.NET Core dans lequel **comptes d’utilisateur individuels** la sécurité est activée. Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* fichier avec la clé `DefaultConnection`. La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas un mot de passe. Pendant le déploiement de l’application, le `DefaultConnection` valeur de clé peut être remplacée par la valeur d’une variable environnement. La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.

> [!WARNING]
> Variables d’environnement sont généralement stockés en texte brut et non chiffré. Si l’ordinateur ou le processus est compromis, variables d’environnement accessibles par des tiers non approuvés. Des mesures supplémentaires pour empêcher la divulgation de secrets de l’utilisateur peuvent être nécessaire.

## <a name="secret-manager"></a>L'outil Secret Manager (Gestionnaire de secrets)

L’outil Gestionnaire de secret principal stocke des données sensibles pendant le développement d’un projet ASP.NET Core. Dans ce contexte, une partie des données sensibles est un secret d’application. Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet. Les secrets de l’application sont associés à un projet spécifique ou partagés entre plusieurs projets. Les secrets de l’application ne sont pas archivés dans le contrôle de code source.

> [!WARNING]
> L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé. Il est utile uniquement à des fins de développement. Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.

## <a name="how-the-secret-manager-tool-works"></a>Fonctionnement de l’outil Secret Manager

L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées. Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation. Les valeurs sont stockées dans un [JSON](https://json.org/) fichier de configuration dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :

* Windows : `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`
* Linux et macOS : `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

Dans l’exemple précédent, les chemins de fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.

Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Gestionnaire de Secret. Ces détails d’implémentation peuvent changer. Par exemple, les valeurs de secret principal ne sont pas chiffrées, mais peut être à l’avenir.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Installer le Gestionnaire de la clé secrète

L’outil Gestionnaire de la clé secrète est fourni avec l’interface de ligne de base .NET dans .NET Core SDK 2.1. .NET Core SDK 2.0 ou version antérieure, l’installation de l’outil est nécessaire.

Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core :

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :

```console
dotnet user-secrets -h
```

L’outil Gestionnaire de Secret affiche l’utilisation de l’exemple, options et l’aide de la commande :

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> Vous devez être dans le même répertoire que le *.csproj* fichier pour exécuter les outils définis dans le *.csproj* du fichier `DotNetCliToolReference` éléments.
::: moniker-end

## <a name="set-a-secret"></a>Définir une clé secrète

L’outil Gestionnaire de Secret opère sur les paramètres de configuration de projet spécifiques stockées dans votre profil utilisateur. Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier. La valeur de `UserSecretsId` est arbitraire, mais est unique au projet. Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel. Cette opération ajoute une `UserSecretsId` élément, rempli avec un GUID, à le *.csproj* fichier. Visual Studio ouvre un *secrets.json* fichier dans l’éditeur de texte. Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker. Par exemple :[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Définir un secret d’application comprenant une clé et sa valeur. Le secret n’est associé au projet `UserSecretsId` valeur. Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.

L’outil Gestionnaire de la clé secrète peut servir à partir d’autres annuaires trop. Utilisez le `--project` option pour indiquer le chemin d’accès de système de fichiers auquel le *.csproj* fichier existe. Par exemple :

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Définir plusieurs clés secrètes

Un lot de secrets peut être défini en transférant JSON à le `set` commande. Dans l’exemple suivant, la *input.json* contenu du fichier est dirigé vers le `set` commande sur Windows :

```console
type .\input.json | dotnet user-secrets set
```

Sur Mac OS et Linux, utilisez la commande suivante :

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a>Accéder à une clé secrète

Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes de gestionnaire de Secret. Si ciblant .NET Core 1.x ou .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.

::: moniker range="<= aspnetcore-1.1"
Ajouter la source de configuration utilisateur secrets pour la `Startup` constructeur :

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Secrets de l’utilisateur peuvent être récupérés via la `Configuration` API :

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Remplacement de chaîne avec les clés secrètes

Il est risqué de stocker des mots de passe en texte brut. Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Une approche plus sécurisée consiste pour stocker le mot de passe en tant que secret. Par exemple :

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Remplacez le mot de passe *appsettings.json* par un espace réservé. Dans l’exemple suivant, `{0}` est utilisé comme espace réservé pour former un [chaîne de Format Composite](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Valeur de la clé secrète peut être injectée dans l’espace réservé pour terminer la chaîne de connexion :

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Répertorier les clés secrètes

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets list
```

La sortie suivante s’affiche :

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

Dans l’exemple précédent, un signe deux-points dans les noms de clé indique au sein de la hiérarchie d’objets *secrets.json*.

## <a name="remove-a-single-secret"></a>Supprimer une clé secrète unique

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

L’application *secrets.json* fichier a été modifié pour supprimer la paire clé-valeur associée à la `MoviesConnectionString` clé :

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

En cours d’exécution `dotnet user-secrets list` affiche le message suivant :

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Supprimez tous les secrets

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets clear
```

Tous les secrets des utilisateurs pour l’application ont été supprimés de la *secrets.json* fichier :

```json
{}
```

En cours d’exécution `dotnet user-secrets list` affiche le message suivant :

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>