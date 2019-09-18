---
title: Stockage sécurisé des secrets d’application en développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets d’application lors du développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080715"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Stockage sécurisé des secrets d’application en développement dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)et [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Ce document explique les techniques permettant de stocker et de récupérer des données sensibles pendant le développement d’une application ASP.NET Core. Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source. Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test. Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables d’environnement

Les variables d’environnement permettent d’éviter le stockage de secrets d’application dans le code ou dans des fichiers de configuration locaux. Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.

::: moniker range="<= aspnetcore-1.1"

Configurez la lecture des valeurs de variable <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> d’environnement `Startup` en appelant dans le constructeur :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Imaginez une application web ASP.NET Core dans laquelle la sécurité **Comptes d’utilisateur individuels** est activée. Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* du projet avec la clé `DefaultConnection`. La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe. Durant le déploiement de l’application, la valeur de la clé `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement. La variable d’environnement peut stocker la chaîne de connexion complète avec des informations d’identification sensibles.

> [!WARNING]
> Les variables d’environnement sont généralement stockées au format texte brut non chiffré. Si l’ordinateur ou le processus est compromis, des tiers non approuvés peuvent y accéder. Il peut donc être nécessaire de prendre des mesures supplémentaires pour empêcher la divulgation des secrets utilisateur.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>L'outil Secret Manager (Gestionnaire de secrets)

L’outil secret Manager stocke les données sensibles pendant le développement d’un projet ASP.NET Core. Dans ce contexte, un élément de données sensibles est un secret d’application. Les secrets de l’application sont stockés dans un emplacement distinct de l’arborescence du projet. Les secrets de l’application sont associés à un projet spécifique ou partagés entre plusieurs projets. Les secrets de l’application ne sont pas archivés dans le contrôle de code source.

> [!WARNING]
> L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé. Il est utile uniquement à des fins de développement. Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.

## <a name="how-the-secret-manager-tool-works"></a>Fonctionnement de l’outil Secret Manager

L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées. Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation. Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégé du système sur l’ordinateur local :

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Chemin d’accès au système de fichiers :

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Chemin d’accès au système de fichiers :

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Dans les chemins d’accès de fichier `<user_secrets_id>` précédents, `UserSecretsId` remplacez par la valeur spécifiée dans le fichier *. csproj* .

N’écrivez pas de code dépendant de l’emplacement ou du format des données enregistrées avec l’outil secret Manager. Ces détails d’implémentation peuvent changer. Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peuvent être dans le futur.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Installer l’outil secret Manager

L’outil Gestionnaire de secret est fourni avec le CLI .NET Core dans kit SDK .NET Core 2.1.300 ou version ultérieure. Pour les versions kit SDK .NET Core antérieures à 2.1.300, l’installation des outils est nécessaire.

> [!TIP]
> Exécutez `dotnet --version` à partir d’une interface de commande pour voir le numéro de version du kit SDK .net Core installé.

Un avertissement s’affiche si le kit SDK .NET Core utilisé comprend l’outil :

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installez le package NuGet [Microsoft. extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) dans votre projet ASP.net core. Exemple :

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Exécutez la commande suivante dans une interface de commande pour valider l’installation de l’outil :

```dotnetcli
dotnet user-secrets -h
```

L’outil Gestionnaire de secret affiche des exemples d’utilisation, d’options et d’aide sur les commandes :

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
> Vous devez être dans le même répertoire que le fichier *. csproj* pour exécuter les outils définis dans les éléments du `DotNetCliToolReference` fichier *. csproj* .

::: moniker-end

## <a name="enable-secret-storage"></a>Activer le stockage secret

L’outil Gestionnaire de secret fonctionne sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.

::: moniker range=">= aspnetcore-3.0"

L’outil Gestionnaire de secret comprend `init` une commande dans kit SDK .net Core 3.0.100 ou version ultérieure. Pour utiliser des secrets d’utilisateur, exécutez la commande suivante dans le répertoire du projet :

```dotnetcli
dotnet user-secrets init
```

La commande précédente ajoute un `UserSecretsId` élément dans un `PropertyGroup` du fichier *. csproj* . Par défaut, le texte interne de `UserSecretsId` est un GUID. Le texte interne est arbitraire, mais il est unique pour le projet.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Pour utiliser des secrets d’utilisateur, `UserSecretsId` définissez un élément `PropertyGroup` dans un du fichier *. csproj* . Le texte interne de `UserSecretsId` est arbitraire, mais il est unique pour le projet. Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> Dans Visual Studio, cliquez avec le bouton droit sur le projet dans Explorateur de solutions, puis sélectionnez **gérer les secrets d’utilisateur** dans le menu contextuel. Ce geste ajoute un `UserSecretsId` élément, rempli avec un GUID, au fichier *. csproj* .

## <a name="set-a-secret"></a>Définir une clé secrète

Définissez un secret d’application comprenant une clé et sa valeur. Le secret est associé à la valeur du `UserSecretsId` projet. Par exemple, exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Dans l’exemple précédent, le signe deux-points indique `Movies` qu’il s’agit d’un `ServiceApiKey` littéral d’objet avec une propriété.

L’outil Gestionnaire de secret peut également être utilisé à partir d’autres annuaires. Utilisez l' `--project` option pour fournir le chemin d’accès au système de fichiers où se trouve le fichier *. csproj* . Par exemple :

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Aplatissement de la structure JSON dans Visual Studio

Le geste **gérer les secrets** de l’utilisateur de Visual Studio ouvre un fichier *secrets. JSON* dans l’éditeur de texte. Remplacez le contenu de *secrets. JSON* par les paires clé-valeur à stocker. Par exemple :

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

La structure JSON est aplatie après des modifications `dotnet user-secrets remove` via `dotnet user-secrets set`ou. Par exemple, l' `dotnet user-secrets remove "Movies:ConnectionString"` exécution de réduit `Movies` le littéral d’objet. Le fichier modifié ressemble à ce qui suit :

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Définir plusieurs secrets

Un lot de secrets peut être défini par la canalisation JSON `set` à la commande. Dans l’exemple suivant, le contenu du fichier *Input. JSON* est dirigé vers la `set` commande.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ouvrez une interface de commande, puis exécutez la commande suivante :

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Ouvrez une interface de commande, puis exécutez la commande suivante :

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Accéder à une clé secrète

L' [API de Configuration ASP.net Core](xref:fundamentals/configuration/index) permet d’accéder aux secrets du gestionnaire de secret.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Si votre projet cible .NET Framework, installez le package NuGet [Microsoft. extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Dans ASP.net Core 2,0 ou version ultérieure, la source de configuration des secrets de l’utilisateur est automatiquement ajoutée en mode <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> de développement lorsque le projet appelle pour initialiser une nouvelle instance de l’hôte avec des valeurs par défaut préconfigurées. `CreateDefaultBuilder`appelle <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> lorsque<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> est :<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Quand `CreateDefaultBuilder` n’est pas appelé, ajoutez explicitement la source de configuration des <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> secrets de `Startup` l’utilisateur en appelant dans le constructeur. Appelez `AddUserSecrets` uniquement lorsque l’application s’exécute dans l’environnement de développement, comme illustré dans l’exemple suivant :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Installez le package NuGet [Microsoft. extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

Ajoutez la source de configuration des secrets de l’utilisateur <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> avec un `Startup` appel à dans le constructeur :

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Les secrets de l’utilisateur peuvent être `Configuration` récupérés via l’API :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Mapper les secrets à un POCO

Le mappage d’un littéral d’objet entier à un POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés associées.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Pour mapper les secrets précédents à un poco, utilisez la `Configuration` fonctionnalité de [liaison de graphique d’objets](xref:fundamentals/configuration/index#bind-to-an-object-graph) de l’API. Le code suivant est lié à un poco `MovieSettings` personnalisé et accède à la `ServiceApiKey` valeur de la propriété :

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

Les `Movies:ConnectionString` secrets `Movies:ServiceApiKey` et sont mappés aux propriétés respectives dans `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Remplacement de chaîne par des secrets

Le stockage des mots de passe en texte brut n’est pas sécurisé. Par exemple, une chaîne de connexion de base de données stockée dans *appSettings. JSON* peut inclure un mot de passe pour l’utilisateur spécifié :

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret. Exemple :

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

Supprimez `Password` la paire clé-valeur de la chaîne de connexion dans *appSettings. JSON*. Par exemple :

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

La valeur du secret peut être définie sur la <xref:System.Data.SqlClient.SqlConnectionStringBuilder> propriété d' <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> un objet pour terminer la chaîne de connexion :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Répertorier les secrets

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :

```dotnetcli
dotnet user-secrets list
```

La sortie suivante apparaît :

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Dans l’exemple précédent, un signe deux-points dans le nom de clé désigne la hiérarchie d’objets dans *secrets. JSON*.

## <a name="remove-a-single-secret"></a>Supprimer une seule clé secrète

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

Le fichier *secrets. JSON* de l’application a été modifié pour supprimer la paire clé-valeur associée `MoviesConnectionString` à la clé :

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

L' `dotnet user-secrets list` exécution affiche le message suivant :

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Supprimer tous les secrets

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :

```dotnetcli
dotnet user-secrets clear
```

Tous les secrets d’utilisateur de l’application ont été supprimés du fichier *secrets. JSON* :

```json
{}
```

L' `dotnet user-secrets list` exécution affiche le message suivant :

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
