---
title: Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets de l’application pendant le développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 195901e466262020fd1217bd9dfb6162910bb861
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893216"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Ce document explique les techniques permettant de stocker et de récupérer des données sensibles pendant le développement d’une application ASP.NET Core. Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source. Aucun secret de production ne doit pas être utilisé pour le développement ou de test. Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables d’environnement

Les variables d’environnement permettent d’éviter le stockage de secrets d’application dans le code ou dans des fichiers de configuration locaux. Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.

::: moniker range="<= aspnetcore-1.1"

Configurer la lecture des valeurs de variable d’environnement en appelant <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> dans le `Startup` constructeur :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Imaginez une application web ASP.NET Core dans laquelle la sécurité **Comptes d’utilisateur individuels** est activée. Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* du projet avec la clé `DefaultConnection`. La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe. Durant le déploiement de l’application, la valeur de la clé `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement. La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.

> [!WARNING]
> Les variables d’environnement sont généralement stockées au format texte brut non chiffré. Si l’ordinateur ou le processus est compromis, des tiers non approuvés peuvent y accéder. Il peut donc être nécessaire de prendre des mesures supplémentaires pour empêcher la divulgation des secrets utilisateur.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>L'outil Secret Manager (Gestionnaire de secrets)

L’outil Secret Manager stocke des données sensibles pendant le développement d’un projet ASP.NET Core. Dans ce contexte, un élément de données sensibles est un secret d’application. Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet. Les secrets d’application sont associés à un projet spécifique ou partagés entre plusieurs projets. Les secrets d’application ne sont pas activés dans le contrôle de code source.

> [!WARNING]
> L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé. Il est utile uniquement à des fins de développement. Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.

## <a name="how-the-secret-manager-tool-works"></a>Fonctionnement de l’outil Secret Manager

L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées. Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation. Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Chemin d’accès du système de fichiers :

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Chemin d’accès du système de fichiers :

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Dans l’exemple précédent, les chemins des fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.

Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Secret Manager. Ces détails d’implémentation peuvent changer. Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peut être à l’avenir.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Installer l’outil Secret Manager

L’outil Secret Manager est fourni avec l’interface CLI .NET Core dans le SDK .NET Core 2.1.300 ou version ultérieure. Pour les versions du SDK .NET Core avant 2.1.300, l’installation de l’outil est nécessaire.

> [!TIP]
> Exécutez `dotnet --version` à partir d’une invite de commandes pour afficher le numéro de version du SDK .NET Core installée.

Un avertissement s’affiche si le SDK .NET Core utilisée inclut l’outil :

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core. Exemple :

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :

```console
dotnet user-secrets -h
```

L’outil Secret Manager affiche des exemples d’utilisation, options et l’aide de la commande :

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

## <a name="enable-secret-storage"></a>Activer le stockage secret

L’outil Secret Manager opère sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.

::: moniker range=">= aspnetcore-3.0"

L’outil Secret Manager inclut un `init` commande dans le SDK .NET Core 3.0.100 ou version ultérieure. Pour utiliser les secrets de l’utilisateur, exécutez la commande suivante dans le répertoire du projet :

```console
dotnet user-secrets init
```

La commande précédente ajoute un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier. Par défaut, le texte interne de `UserSecretsId` est un GUID. Le texte interne est arbitraire, mais il est unique au projet.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier. Le texte interne de `UserSecretsId` est arbitraire, mais est unique au projet. Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel. Ce mouvement ajoute un `UserSecretsId` élément, rempli avec un GUID, à la *.csproj* fichier.

## <a name="set-a-secret"></a>Définir une clé secrète

Définir une clé secrète d’application composée d’une clé et sa valeur. Le secret est associé avec le projet `UserSecretsId` valeur. Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.

L’outil Secret Manager peut être utilisé à partir d’autres répertoires. Utilisez le `--project` option pour fournir le chemin d’accès de système de fichier à partir duquel le *.csproj* fichier existe. Exemple :

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Structure JSON mise à plat dans Visual Studio

De Visual Studio **gérer les Secrets utilisateur** mouvements ouvre un *secrets.json* fichier dans l’éditeur de texte. Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker. Exemple :

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

La structure JSON est aplatie après les modifications opérées par le biais de `dotnet user-secrets remove` ou `dotnet user-secrets set`. Par exemple, en cours d’exécution `dotnet user-secrets remove "Movies:ConnectionString"` réduit le `Movies` littéral d’objet. Le fichier modifié ressemble à ceci :

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Définir plusieurs clés secrètes

Un lot de secrets peut être défini en dirigeant JSON pour le `set` commande. Dans l’exemple suivant, le *input.json* contenu du fichier est dirigés vers le `set` commande.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ouvrez une invite de commandes et exécutez la commande suivante :

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Ouvrez une invite de commandes et exécutez la commande suivante :

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Accéder à une clé secrète

Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Si votre projet cible .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Dans ASP.NET Core 2.0 ou version ultérieure, la source de configuration de secrets utilisateur est automatiquement ajoutée en mode de développement lorsque le projet appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour initialiser une nouvelle instance de l’hôte avec les valeurs par défaut préconfigurés. `CreateDefaultBuilder` appels <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> lorsque le <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> est <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Lorsque `CreateDefaultBuilder` n’est pas appelée, ajouter la source de configuration de secrets utilisateur explicitement en appelant <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> dans le `Startup` constructeur. Appelez `AddUserSecrets` uniquement lorsque l’application s’exécute dans l’environnement de développement, comme illustré dans l’exemple suivant :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Installer le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.

Ajouter la source de configuration de secrets utilisateur avec un appel à <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> dans le `Startup` constructeur :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Secrets de l’utilisateur peuvent être récupérées via la `Configuration` API :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Mapper des secrets à un objet POCO

Mappage d’un littéral d’objet entier à un objet POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés connexes.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Pour mapper les secrets précédentes à un objet POCO, utilisez le `Configuration` API [de liaison du graphique d’objet](xref:fundamentals/configuration/index#bind-to-an-object-graph) fonctionnalité. Le code suivant lie un personnalisé `MovieSettings` POCO et accède à la `ServiceApiKey` valeur de propriété :

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

Le `Movies:ConnectionString` et `Movies:ServiceApiKey` secrets sont mappées aux propriétés respectives dans `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Remplacement de chaîne avec des secrets

Stocker les mots de passe en texte brut n’est pas sécurisé. Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret. Exemple :

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Supprimer le `Password` paire clé-valeur à partir de la chaîne de connexion dans *appsettings.json*. Exemple :

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Valeur du secret peut être définie sur une <xref:System.Data.SqlClient.SqlConnectionStringBuilder> l’objet <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> propriété pour terminer la chaîne de connexion :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Répertorier les clés secrètes

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets list
```

La sortie suivante apparaît :

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Dans l’exemple précédent, un signe deux-points dans les noms de clé désigne la hiérarchie d’objets au sein de *secrets.json*.

## <a name="remove-a-single-secret"></a>Supprimer un secret unique

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

## <a name="remove-all-secrets"></a>Supprimer tous les secrets

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :

```console
dotnet user-secrets clear
```

Tous les secrets d’utilisateur pour l’application ont été supprimés de la *secrets.json* fichier :

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
