---
title: Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets de l’application pendant le développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2019
uid: security/app-secrets
ms.openlocfilehash: eaa2e9d1ba98d391a29a9ff55872d062df016b87
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667776"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="5d956-103">Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5d956-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="5d956-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5d956-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5d956-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5d956-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5d956-106">Ce document explique les techniques permettant de stocker et de récupérer des données sensibles pendant le développement d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d956-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="5d956-107">Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="5d956-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="5d956-108">Aucun secret de production ne doit pas être utilisé pour le développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="5d956-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="5d956-109">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="5d956-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="5d956-110">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="5d956-110">Environment variables</span></span>

<span data-ttu-id="5d956-111">Les variables d’environnement permettent d’éviter le stockage de secrets d’application dans le code ou dans des fichiers de configuration locaux.</span><span class="sxs-lookup"><span data-stu-id="5d956-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="5d956-112">Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.</span><span class="sxs-lookup"><span data-stu-id="5d956-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5d956-113">Configurer la lecture des valeurs de variable d’environnement en appelant [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="5d956-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="5d956-114">Imaginez une application web ASP.NET Core dans laquelle la sécurité **Comptes d’utilisateur individuels** est activée.</span><span class="sxs-lookup"><span data-stu-id="5d956-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="5d956-115">Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* du projet avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="5d956-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="5d956-116">La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="5d956-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="5d956-117">Durant le déploiement de l’application, la valeur de la clé `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="5d956-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="5d956-118">La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.</span><span class="sxs-lookup"><span data-stu-id="5d956-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="5d956-119">Les variables d’environnement sont généralement stockées au format texte brut non chiffré.</span><span class="sxs-lookup"><span data-stu-id="5d956-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="5d956-120">Si l’ordinateur ou le processus est compromis, des tiers non approuvés peuvent y accéder.</span><span class="sxs-lookup"><span data-stu-id="5d956-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="5d956-121">Il peut donc être nécessaire de prendre des mesures supplémentaires pour empêcher la divulgation des secrets utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5d956-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="5d956-122">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="5d956-122">Secret Manager</span></span>

<span data-ttu-id="5d956-123">L’outil Secret Manager stocke des données sensibles pendant le développement d’un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d956-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="5d956-124">Dans ce contexte, un élément de données sensibles est un secret d’application.</span><span class="sxs-lookup"><span data-stu-id="5d956-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="5d956-125">Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet.</span><span class="sxs-lookup"><span data-stu-id="5d956-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="5d956-126">Les secrets d’application sont associés à un projet spécifique ou partagés entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="5d956-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="5d956-127">Les secrets d’application ne sont pas activés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="5d956-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="5d956-128">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="5d956-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="5d956-129">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="5d956-129">It's for development purposes only.</span></span> <span data-ttu-id="5d956-130">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5d956-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="5d956-131">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="5d956-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="5d956-132">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="5d956-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="5d956-133">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="5d956-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="5d956-134">Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="5d956-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5d956-135">Windows</span><span class="sxs-lookup"><span data-stu-id="5d956-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="5d956-136">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="5d956-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="5d956-137">macOS</span><span class="sxs-lookup"><span data-stu-id="5d956-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="5d956-138">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="5d956-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="5d956-139">Linux</span><span class="sxs-lookup"><span data-stu-id="5d956-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="5d956-140">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="5d956-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="5d956-141">Dans l’exemple précédent, les chemins des fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="5d956-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="5d956-142">Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="5d956-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="5d956-143">Ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="5d956-143">These implementation details may change.</span></span> <span data-ttu-id="5d956-144">Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peut être à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="5d956-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="5d956-145">Installer l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="5d956-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="5d956-146">L’outil Secret Manager est fourni avec l’interface CLI .NET Core dans le SDK .NET Core 2.1.300 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="5d956-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="5d956-147">Pour les versions du SDK .NET Core avant 2.1.300, l’installation de l’outil est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="5d956-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="5d956-148">Exécutez `dotnet --version` à partir d’une invite de commandes pour afficher le numéro de version du SDK .NET Core installée.</span><span class="sxs-lookup"><span data-stu-id="5d956-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="5d956-149">Un avertissement s’affiche si le SDK .NET Core utilisée inclut l’outil :</span><span class="sxs-lookup"><span data-stu-id="5d956-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="5d956-150">Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5d956-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="5d956-151">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5d956-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="5d956-152">Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :</span><span class="sxs-lookup"><span data-stu-id="5d956-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="5d956-153">L’outil Secret Manager affiche des exemples d’utilisation, options et l’aide de la commande :</span><span class="sxs-lookup"><span data-stu-id="5d956-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="5d956-154">Vous devez être dans le même répertoire que le *.csproj* fichier pour exécuter les outils définis dans le *.csproj* du fichier `DotNetCliToolReference` éléments.</span><span class="sxs-lookup"><span data-stu-id="5d956-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="5d956-155">Définir une clé secrète</span><span class="sxs-lookup"><span data-stu-id="5d956-155">Set a secret</span></span>

<span data-ttu-id="5d956-156">L’outil Secret Manager opère sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5d956-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="5d956-157">Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="5d956-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="5d956-158">La valeur de `UserSecretsId` est arbitraire, mais est unique au projet.</span><span class="sxs-lookup"><span data-stu-id="5d956-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="5d956-159">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="5d956-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="5d956-160">Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="5d956-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="5d956-161">Ce mouvement ajoute un `UserSecretsId` élément, rempli avec un GUID, à la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="5d956-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="5d956-162">Visual Studio ouvre un *secrets.json* fichier dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="5d956-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="5d956-163">Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker.</span><span class="sxs-lookup"><span data-stu-id="5d956-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="5d956-164">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5d956-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="5d956-165">La structure JSON est aplatie après les modifications opérées par le biais de `dotnet user-secrets remove` ou `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="5d956-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="5d956-166">Par exemple, en cours d’exécution `dotnet user-secrets remove "Movies:ConnectionString"` réduit le `Movies` littéral d’objet.</span><span class="sxs-lookup"><span data-stu-id="5d956-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="5d956-167">Le fichier modifié ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="5d956-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="5d956-168">Définir une clé secrète d’application composée d’une clé et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="5d956-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="5d956-169">Le secret est associé avec le projet `UserSecretsId` valeur.</span><span class="sxs-lookup"><span data-stu-id="5d956-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="5d956-170">Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="5d956-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="5d956-171">Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.</span><span class="sxs-lookup"><span data-stu-id="5d956-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="5d956-172">L’outil Secret Manager peut être utilisé à partir d’autres répertoires.</span><span class="sxs-lookup"><span data-stu-id="5d956-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="5d956-173">Utilisez le `--project` option pour fournir le chemin d’accès de système de fichier à partir duquel le *.csproj* fichier existe.</span><span class="sxs-lookup"><span data-stu-id="5d956-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="5d956-174">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5d956-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="5d956-175">Définir plusieurs clés secrètes</span><span class="sxs-lookup"><span data-stu-id="5d956-175">Set multiple secrets</span></span>

<span data-ttu-id="5d956-176">Un lot de secrets peut être défini en dirigeant JSON pour le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="5d956-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="5d956-177">Dans l’exemple suivant, le *input.json* contenu du fichier est dirigés vers le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="5d956-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="5d956-178">Windows</span><span class="sxs-lookup"><span data-stu-id="5d956-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="5d956-179">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5d956-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="5d956-180">macOS</span><span class="sxs-lookup"><span data-stu-id="5d956-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="5d956-181">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5d956-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="5d956-182">Linux</span><span class="sxs-lookup"><span data-stu-id="5d956-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="5d956-183">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5d956-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="5d956-184">Accéder à une clé secrète</span><span class="sxs-lookup"><span data-stu-id="5d956-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5d956-185">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="5d956-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="5d956-186">Si votre projet cible le .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d956-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="5d956-187">Dans ASP.NET Core 2.0 ou version ultérieure, la source de configuration de secrets utilisateur est automatiquement ajoutée en mode de développement lorsque le projet appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour initialiser une nouvelle instance de l’hôte avec les valeurs par défaut préconfigurés.</span><span class="sxs-lookup"><span data-stu-id="5d956-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="5d956-188">`CreateDefaultBuilder` appels <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> lorsque le <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> est <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="5d956-188">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="5d956-189">Lorsque `CreateDefaultBuilder` n’est pas appelée, ajouter la source de configuration de secrets utilisateur explicitement en appelant <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> dans le `Startup` constructeur.</span><span class="sxs-lookup"><span data-stu-id="5d956-189">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="5d956-190">Appelez `AddUserSecrets` uniquement lorsque l’application s’exécute dans l’environnement de développement, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="5d956-190">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="5d956-191">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="5d956-191">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="5d956-192">Installer le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="5d956-192">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="5d956-193">Ajouter la source de configuration de secrets utilisateur avec un appel à [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="5d956-193">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="5d956-194">Secrets de l’utilisateur peuvent être récupérées via la `Configuration` API :</span><span class="sxs-lookup"><span data-stu-id="5d956-194">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="5d956-195">Mapper des secrets à un objet POCO</span><span class="sxs-lookup"><span data-stu-id="5d956-195">Map secrets to a POCO</span></span>

<span data-ttu-id="5d956-196">Mappage d’un littéral d’objet entier à un objet POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés connexes.</span><span class="sxs-lookup"><span data-stu-id="5d956-196">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5d956-197">Pour mapper les secrets précédentes à un objet POCO, utilisez le `Configuration` API [de liaison du graphique d’objet](xref:fundamentals/configuration/index#bind-to-an-object-graph) fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="5d956-197">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="5d956-198">Le code suivant lie un personnalisé `MovieSettings` POCO et accède à la `ServiceApiKey` valeur de propriété :</span><span class="sxs-lookup"><span data-stu-id="5d956-198">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="5d956-199">Le `Movies:ConnectionString` et `Movies:ServiceApiKey` secrets sont mappées aux propriétés respectives dans `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="5d956-199">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="5d956-200">Remplacement de chaîne avec des secrets</span><span class="sxs-lookup"><span data-stu-id="5d956-200">String replacement with secrets</span></span>

<span data-ttu-id="5d956-201">Stocker les mots de passe en texte brut n’est pas sécurisé.</span><span class="sxs-lookup"><span data-stu-id="5d956-201">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="5d956-202">Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :</span><span class="sxs-lookup"><span data-stu-id="5d956-202">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="5d956-203">Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret.</span><span class="sxs-lookup"><span data-stu-id="5d956-203">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="5d956-204">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5d956-204">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="5d956-205">Supprimer le `Password` paire clé-valeur à partir de la chaîne de connexion dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5d956-205">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="5d956-206">Exemple :</span><span class="sxs-lookup"><span data-stu-id="5d956-206">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="5d956-207">Valeur du secret peut être définie sur une [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) l’objet [mot de passe](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propriété pour terminer la chaîne de connexion :</span><span class="sxs-lookup"><span data-stu-id="5d956-207">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="5d956-208">Répertorier les clés secrètes</span><span class="sxs-lookup"><span data-stu-id="5d956-208">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5d956-209">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="5d956-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="5d956-210">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="5d956-210">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="5d956-211">Dans l’exemple précédent, un signe deux-points dans les noms de clé désigne la hiérarchie d’objets au sein de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="5d956-211">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="5d956-212">Supprimer un secret unique</span><span class="sxs-lookup"><span data-stu-id="5d956-212">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5d956-213">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="5d956-213">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="5d956-214">L’application *secrets.json* fichier a été modifié pour supprimer la paire clé-valeur associée à la `MoviesConnectionString` clé :</span><span class="sxs-lookup"><span data-stu-id="5d956-214">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="5d956-215">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="5d956-215">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="5d956-216">Supprimer tous les secrets</span><span class="sxs-lookup"><span data-stu-id="5d956-216">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="5d956-217">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="5d956-217">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="5d956-218">Tous les secrets d’utilisateur pour l’application ont été supprimés de la *secrets.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="5d956-218">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="5d956-219">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="5d956-219">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="5d956-220">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5d956-220">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
