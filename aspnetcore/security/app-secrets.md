---
title: Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets de l’application pendant le développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41836257"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="52218-103">Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52218-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="52218-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="52218-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="52218-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52218-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="52218-106">Ce document explique les techniques permettant de stocker et de récupérer des données sensibles pendant le développement d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52218-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="52218-107">Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source, et n'utilisez pas de secrets de fabrication en mode développement ou test.</span><span class="sxs-lookup"><span data-stu-id="52218-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="52218-108">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="52218-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="52218-109">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="52218-109">Environment variables</span></span>

<span data-ttu-id="52218-110">Les variables d’environnement permettent d’éviter le stockage de secrets d’application dans le code ou dans des fichiers de configuration locaux.</span><span class="sxs-lookup"><span data-stu-id="52218-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="52218-111">Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.</span><span class="sxs-lookup"><span data-stu-id="52218-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="52218-112">Configurer la lecture des valeurs de variable d’environnement en appelant [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="52218-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="52218-113">Imaginez une application web ASP.NET Core dans laquelle la sécurité **Comptes d’utilisateur individuels** est activée.</span><span class="sxs-lookup"><span data-stu-id="52218-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="52218-114">Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* du projet avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="52218-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="52218-115">La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="52218-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="52218-116">Durant le déploiement de l’application, la valeur de la clé `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="52218-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="52218-117">La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.</span><span class="sxs-lookup"><span data-stu-id="52218-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="52218-118">Les variables d’environnement sont généralement stockées au format texte brut non chiffré.</span><span class="sxs-lookup"><span data-stu-id="52218-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="52218-119">Si l’ordinateur ou le processus est compromis, des tiers non approuvés peuvent y accéder.</span><span class="sxs-lookup"><span data-stu-id="52218-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="52218-120">Il peut donc être nécessaire de prendre des mesures supplémentaires pour empêcher la divulgation des secrets utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52218-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="52218-121">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="52218-121">Secret Manager</span></span>

<span data-ttu-id="52218-122">L’outil Secret Manager stocke des données sensibles pendant le développement d’un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52218-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="52218-123">Dans ce contexte, un élément de données sensibles est un secret d’application.</span><span class="sxs-lookup"><span data-stu-id="52218-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="52218-124">Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet.</span><span class="sxs-lookup"><span data-stu-id="52218-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="52218-125">Les secrets d’application sont associés à un projet spécifique ou partagés entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="52218-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="52218-126">Les secrets d’application ne sont pas activés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="52218-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="52218-127">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="52218-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="52218-128">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="52218-128">It's for development purposes only.</span></span> <span data-ttu-id="52218-129">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52218-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="52218-130">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="52218-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="52218-131">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="52218-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="52218-132">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="52218-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="52218-133">Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="52218-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="52218-134">Windows</span><span class="sxs-lookup"><span data-stu-id="52218-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="52218-135">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="52218-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="52218-136">macOS</span><span class="sxs-lookup"><span data-stu-id="52218-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="52218-137">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="52218-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="52218-138">Linux</span><span class="sxs-lookup"><span data-stu-id="52218-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="52218-139">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="52218-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="52218-140">Dans l’exemple précédent, les chemins des fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="52218-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="52218-141">Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="52218-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="52218-142">Ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="52218-142">These implementation details may change.</span></span> <span data-ttu-id="52218-143">Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peut être à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="52218-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="52218-144">Installer l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="52218-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="52218-145">L’outil Secret Manager est fourni avec l’interface CLI .NET Core dans le SDK .NET Core 2.1.300 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="52218-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="52218-146">Pour les versions du SDK .NET Core avant 2.1.300, l’installation de l’outil est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="52218-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="52218-147">Exécutez `dotnet --version` à partir d’une invite de commandes pour afficher le numéro de version du SDK .NET Core installée.</span><span class="sxs-lookup"><span data-stu-id="52218-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="52218-148">Un avertissement s’affiche si le SDK .NET Core utilisée inclut l’outil :</span><span class="sxs-lookup"><span data-stu-id="52218-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="52218-149">Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="52218-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="52218-150">Exemple :</span><span class="sxs-lookup"><span data-stu-id="52218-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="52218-151">Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :</span><span class="sxs-lookup"><span data-stu-id="52218-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="52218-152">L’outil Secret Manager affiche des exemples d’utilisation, options et l’aide de la commande :</span><span class="sxs-lookup"><span data-stu-id="52218-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="52218-153">Vous devez être dans le même répertoire que le *.csproj* fichier pour exécuter les outils définis dans le *.csproj* du fichier `DotNetCliToolReference` éléments.</span><span class="sxs-lookup"><span data-stu-id="52218-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="52218-154">Définir une clé secrète</span><span class="sxs-lookup"><span data-stu-id="52218-154">Set a secret</span></span>

<span data-ttu-id="52218-155">L’outil Secret Manager opère sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="52218-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="52218-156">Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="52218-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="52218-157">La valeur de `UserSecretsId` est arbitraire, mais est unique au projet.</span><span class="sxs-lookup"><span data-stu-id="52218-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="52218-158">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="52218-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="52218-159">Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="52218-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="52218-160">Ce mouvement ajoute un `UserSecretsId` élément, rempli avec un GUID, à la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="52218-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="52218-161">Visual Studio ouvre un *secrets.json* fichier dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="52218-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="52218-162">Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker.</span><span class="sxs-lookup"><span data-stu-id="52218-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="52218-163">Exemple :</span><span class="sxs-lookup"><span data-stu-id="52218-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="52218-164">La structure JSON est aplatie après les modifications opérées par le biais de `dotnet user-secrets remove` ou `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="52218-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="52218-165">Par exemple, en cours d’exécution `dotnet user-secrets remove "Movies:ConnectionString"` réduit le `Movies` littéral d’objet.</span><span class="sxs-lookup"><span data-stu-id="52218-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="52218-166">Le fichier modifié ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="52218-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="52218-167">Définir une clé secrète d’application composée d’une clé et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="52218-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="52218-168">Le secret est associé avec le projet `UserSecretsId` valeur.</span><span class="sxs-lookup"><span data-stu-id="52218-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="52218-169">Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="52218-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="52218-170">Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.</span><span class="sxs-lookup"><span data-stu-id="52218-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="52218-171">L’outil Secret Manager peut être utilisé à partir d’autres répertoires.</span><span class="sxs-lookup"><span data-stu-id="52218-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="52218-172">Utilisez le `--project` option pour fournir le chemin d’accès de système de fichier à partir duquel le *.csproj* fichier existe.</span><span class="sxs-lookup"><span data-stu-id="52218-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="52218-173">Exemple :</span><span class="sxs-lookup"><span data-stu-id="52218-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="52218-174">Définir plusieurs clés secrètes</span><span class="sxs-lookup"><span data-stu-id="52218-174">Set multiple secrets</span></span>

<span data-ttu-id="52218-175">Un lot de secrets peut être défini en dirigeant JSON pour le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="52218-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="52218-176">Dans l’exemple suivant, le *input.json* contenu du fichier est dirigés vers le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="52218-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="52218-177">Windows</span><span class="sxs-lookup"><span data-stu-id="52218-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="52218-178">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="52218-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="52218-179">macOS</span><span class="sxs-lookup"><span data-stu-id="52218-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="52218-180">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="52218-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="52218-181">Linux</span><span class="sxs-lookup"><span data-stu-id="52218-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="52218-182">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="52218-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="52218-183">Accéder à une clé secrète</span><span class="sxs-lookup"><span data-stu-id="52218-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="52218-184">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="52218-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="52218-185">Si votre projet cible le .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="52218-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="52218-186">Dans ASP.NET Core 2.0 ou version ultérieure, la source de configuration de secrets utilisateur est automatiquement ajoutée en mode de développement lorsque le projet appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour initialiser une nouvelle instance de l’hôte avec les valeurs par défaut préconfigurés.</span><span class="sxs-lookup"><span data-stu-id="52218-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="52218-187">`CreateDefaultBuilder` appels [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) lorsque le [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) est [développement](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="52218-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="52218-188">Lorsque `CreateDefaultBuilder` n’est pas appelé pendant la construction de l’hôte, ajoutez la source de configuration de secrets utilisateur avec un appel à [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="52218-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="52218-189">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="52218-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="52218-190">Installer le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="52218-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="52218-191">Ajouter la source de configuration de secrets utilisateur avec un appel à [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="52218-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="52218-192">Secrets de l’utilisateur peuvent être récupérées via la `Configuration` API :</span><span class="sxs-lookup"><span data-stu-id="52218-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="52218-193">Mapper des secrets à un objet POCO</span><span class="sxs-lookup"><span data-stu-id="52218-193">Map secrets to a POCO</span></span>

<span data-ttu-id="52218-194">Mappage d’un littéral d’objet entier à un objet POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés connexes.</span><span class="sxs-lookup"><span data-stu-id="52218-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="52218-195">Pour mapper les secrets précédentes à un objet POCO, utilisez le `Configuration` API [de liaison du graphique d’objet](xref:fundamentals/configuration/index#bind-to-an-object-graph) fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="52218-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="52218-196">Le code suivant lie un personnalisé `MovieSettings` POCO et accède à la `ServiceApiKey` valeur de propriété :</span><span class="sxs-lookup"><span data-stu-id="52218-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="52218-197">Le `Movies:ConnectionString` et `Movies:ServiceApiKey` secrets sont mappées aux propriétés respectives dans `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="52218-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="52218-198">Remplacement de chaîne avec des secrets</span><span class="sxs-lookup"><span data-stu-id="52218-198">String replacement with secrets</span></span>

<span data-ttu-id="52218-199">Stocker les mots de passe en texte brut n’est pas sécurisé.</span><span class="sxs-lookup"><span data-stu-id="52218-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="52218-200">Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :</span><span class="sxs-lookup"><span data-stu-id="52218-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="52218-201">Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret.</span><span class="sxs-lookup"><span data-stu-id="52218-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="52218-202">Exemple :</span><span class="sxs-lookup"><span data-stu-id="52218-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="52218-203">Supprimer le `Password` paire clé-valeur à partir de la chaîne de connexion dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="52218-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="52218-204">Exemple :</span><span class="sxs-lookup"><span data-stu-id="52218-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="52218-205">Valeur du secret peut être définie sur une [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) l’objet [mot de passe](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propriété pour terminer la chaîne de connexion :</span><span class="sxs-lookup"><span data-stu-id="52218-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="52218-206">Répertorier les clés secrètes</span><span class="sxs-lookup"><span data-stu-id="52218-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="52218-207">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="52218-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="52218-208">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="52218-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="52218-209">Dans l’exemple précédent, un signe deux-points dans les noms de clé désigne la hiérarchie d’objets au sein de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="52218-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="52218-210">Supprimer un secret unique</span><span class="sxs-lookup"><span data-stu-id="52218-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="52218-211">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="52218-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="52218-212">L’application *secrets.json* fichier a été modifié pour supprimer la paire clé-valeur associée à la `MoviesConnectionString` clé :</span><span class="sxs-lookup"><span data-stu-id="52218-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="52218-213">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="52218-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="52218-214">Supprimer tous les secrets</span><span class="sxs-lookup"><span data-stu-id="52218-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="52218-215">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="52218-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="52218-216">Tous les secrets d’utilisateur pour l’application ont été supprimés de la *secrets.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="52218-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="52218-217">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="52218-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="52218-218">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="52218-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
