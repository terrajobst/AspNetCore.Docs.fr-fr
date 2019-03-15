---
title: Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets de l’application pendant le développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 1a10c4d035510c689e3eccadc5986df0cc06b71e
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841512"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="eadd6-103">Stockage sécurisé des secrets d’application dans le développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eadd6-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="eadd6-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="eadd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="eadd6-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eadd6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eadd6-106">Ce document explique les techniques permettant de stocker et de récupérer des données sensibles pendant le développement d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eadd6-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="eadd6-107">Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="eadd6-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="eadd6-108">Aucun secret de production ne doit pas être utilisé pour le développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="eadd6-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="eadd6-109">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="eadd6-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="eadd6-110">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="eadd6-110">Environment variables</span></span>

<span data-ttu-id="eadd6-111">Les variables d’environnement permettent d’éviter le stockage de secrets d’application dans le code ou dans des fichiers de configuration locaux.</span><span class="sxs-lookup"><span data-stu-id="eadd6-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="eadd6-112">Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.</span><span class="sxs-lookup"><span data-stu-id="eadd6-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eadd6-113">Configurer la lecture des valeurs de variable d’environnement en appelant <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="eadd6-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="eadd6-114">Imaginez une application web ASP.NET Core dans laquelle la sécurité **Comptes d’utilisateur individuels** est activée.</span><span class="sxs-lookup"><span data-stu-id="eadd6-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="eadd6-115">Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* du projet avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="eadd6-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="eadd6-116">La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="eadd6-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="eadd6-117">Durant le déploiement de l’application, la valeur de la clé `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="eadd6-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="eadd6-118">La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.</span><span class="sxs-lookup"><span data-stu-id="eadd6-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="eadd6-119">Les variables d’environnement sont généralement stockées au format texte brut non chiffré.</span><span class="sxs-lookup"><span data-stu-id="eadd6-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="eadd6-120">Si l’ordinateur ou le processus est compromis, des tiers non approuvés peuvent y accéder.</span><span class="sxs-lookup"><span data-stu-id="eadd6-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="eadd6-121">Il peut donc être nécessaire de prendre des mesures supplémentaires pour empêcher la divulgation des secrets utilisateur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="eadd6-122">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="eadd6-122">Secret Manager</span></span>

<span data-ttu-id="eadd6-123">L’outil Secret Manager stocke des données sensibles pendant le développement d’un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eadd6-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="eadd6-124">Dans ce contexte, un élément de données sensibles est un secret d’application.</span><span class="sxs-lookup"><span data-stu-id="eadd6-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="eadd6-125">Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="eadd6-126">Les secrets d’application sont associés à un projet spécifique ou partagés entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="eadd6-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="eadd6-127">Les secrets d’application ne sont pas activés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="eadd6-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="eadd6-128">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="eadd6-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="eadd6-129">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="eadd6-129">It's for development purposes only.</span></span> <span data-ttu-id="eadd6-130">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="eadd6-131">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="eadd6-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="eadd6-132">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="eadd6-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="eadd6-133">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="eadd6-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="eadd6-134">Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="eadd6-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eadd6-135">Windows</span><span class="sxs-lookup"><span data-stu-id="eadd6-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="eadd6-136">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="eadd6-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="eadd6-137">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="eadd6-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="eadd6-138">Chemin d’accès du système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="eadd6-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="eadd6-139">Dans l’exemple précédent, les chemins des fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="eadd6-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="eadd6-140">Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="eadd6-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="eadd6-141">Ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="eadd6-141">These implementation details may change.</span></span> <span data-ttu-id="eadd6-142">Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peut être à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="eadd6-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="eadd6-143">Installer l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="eadd6-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="eadd6-144">L’outil Secret Manager est fourni avec l’interface CLI .NET Core dans le SDK .NET Core 2.1.300 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="eadd6-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="eadd6-145">Pour les versions du SDK .NET Core avant 2.1.300, l’installation de l’outil est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="eadd6-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="eadd6-146">Exécutez `dotnet --version` à partir d’une invite de commandes pour afficher le numéro de version du SDK .NET Core installée.</span><span class="sxs-lookup"><span data-stu-id="eadd6-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="eadd6-147">Un avertissement s’affiche si le SDK .NET Core utilisée inclut l’outil :</span><span class="sxs-lookup"><span data-stu-id="eadd6-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="eadd6-148">Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eadd6-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="eadd6-149">Exemple :</span><span class="sxs-lookup"><span data-stu-id="eadd6-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="eadd6-150">Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :</span><span class="sxs-lookup"><span data-stu-id="eadd6-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="eadd6-151">L’outil Secret Manager affiche des exemples d’utilisation, options et l’aide de la commande :</span><span class="sxs-lookup"><span data-stu-id="eadd6-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="eadd6-152">Vous devez être dans le même répertoire que le *.csproj* fichier pour exécuter les outils définis dans le *.csproj* du fichier `DotNetCliToolReference` éléments.</span><span class="sxs-lookup"><span data-stu-id="eadd6-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="eadd6-153">Activer le stockage secret</span><span class="sxs-lookup"><span data-stu-id="eadd6-153">Enable secret storage</span></span>

<span data-ttu-id="eadd6-154">L’outil Secret Manager opère sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="eadd6-155">L’outil Secret Manager inclut un `init` commande dans le SDK .NET Core 3.0.100 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="eadd6-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="eadd6-156">Pour utiliser les secrets de l’utilisateur, exécutez la commande suivante dans le répertoire du projet :</span><span class="sxs-lookup"><span data-stu-id="eadd6-156">To use user secrets, run the following command in the project directory:</span></span>

```console
dotnet user-secrets init
```

<span data-ttu-id="eadd6-157">La commande précédente ajoute un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="eadd6-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="eadd6-158">Par défaut, le texte interne de `UserSecretsId` est un GUID.</span><span class="sxs-lookup"><span data-stu-id="eadd6-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="eadd6-159">Le texte interne est arbitraire, mais il est unique au projet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="eadd6-160">Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="eadd6-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="eadd6-161">Le texte interne de `UserSecretsId` est arbitraire, mais est unique au projet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="eadd6-162">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="eadd6-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="eadd6-163">Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="eadd6-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="eadd6-164">Ce mouvement ajoute un `UserSecretsId` élément, rempli avec un GUID, à la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="eadd6-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="eadd6-165">Définir une clé secrète</span><span class="sxs-lookup"><span data-stu-id="eadd6-165">Set a secret</span></span>

<span data-ttu-id="eadd6-166">Définir une clé secrète d’application composée d’une clé et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="eadd6-167">Le secret est associé avec le projet `UserSecretsId` valeur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="eadd6-168">Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="eadd6-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="eadd6-169">Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.</span><span class="sxs-lookup"><span data-stu-id="eadd6-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="eadd6-170">L’outil Secret Manager peut être utilisé à partir d’autres répertoires.</span><span class="sxs-lookup"><span data-stu-id="eadd6-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="eadd6-171">Utilisez le `--project` option pour fournir le chemin d’accès de système de fichier à partir duquel le *.csproj* fichier existe.</span><span class="sxs-lookup"><span data-stu-id="eadd6-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="eadd6-172">Exemple :</span><span class="sxs-lookup"><span data-stu-id="eadd6-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="eadd6-173">Structure JSON mise à plat dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eadd6-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="eadd6-174">De Visual Studio **gérer les Secrets utilisateur** mouvements ouvre un *secrets.json* fichier dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="eadd6-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="eadd6-175">Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker.</span><span class="sxs-lookup"><span data-stu-id="eadd6-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="eadd6-176">Exemple :</span><span class="sxs-lookup"><span data-stu-id="eadd6-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="eadd6-177">La structure JSON est aplatie après les modifications opérées par le biais de `dotnet user-secrets remove` ou `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="eadd6-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="eadd6-178">Par exemple, en cours d’exécution `dotnet user-secrets remove "Movies:ConnectionString"` réduit le `Movies` littéral d’objet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="eadd6-179">Le fichier modifié ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="eadd6-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="eadd6-180">Définir plusieurs clés secrètes</span><span class="sxs-lookup"><span data-stu-id="eadd6-180">Set multiple secrets</span></span>

<span data-ttu-id="eadd6-181">Un lot de secrets peut être défini en dirigeant JSON pour le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="eadd6-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="eadd6-182">Dans l’exemple suivant, le *input.json* contenu du fichier est dirigés vers le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="eadd6-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eadd6-183">Windows</span><span class="sxs-lookup"><span data-stu-id="eadd6-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="eadd6-184">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="eadd6-184">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="eadd6-185">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="eadd6-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="eadd6-186">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="eadd6-186">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="eadd6-187">Accéder à une clé secrète</span><span class="sxs-lookup"><span data-stu-id="eadd6-187">Access a secret</span></span>

<span data-ttu-id="eadd6-188">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="eadd6-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="eadd6-189">Si votre projet cible .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="eadd6-190">Dans ASP.NET Core 2.0 ou version ultérieure, la source de configuration de secrets utilisateur est automatiquement ajoutée en mode de développement lorsque le projet appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> pour initialiser une nouvelle instance de l’hôte avec les valeurs par défaut préconfigurés.</span><span class="sxs-lookup"><span data-stu-id="eadd6-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="eadd6-191">`CreateDefaultBuilder` appels <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> lorsque le <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> est <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="eadd6-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="eadd6-192">Lorsque `CreateDefaultBuilder` n’est pas appelée, ajouter la source de configuration de secrets utilisateur explicitement en appelant <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> dans le `Startup` constructeur.</span><span class="sxs-lookup"><span data-stu-id="eadd6-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="eadd6-193">Appelez `AddUserSecrets` uniquement lorsque l’application s’exécute dans l’environnement de développement, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="eadd6-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eadd6-194">Installer le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="eadd6-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="eadd6-195">Ajouter la source de configuration de secrets utilisateur avec un appel à <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="eadd6-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="eadd6-196">Secrets de l’utilisateur peuvent être récupérées via la `Configuration` API :</span><span class="sxs-lookup"><span data-stu-id="eadd6-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="eadd6-197">Mapper des secrets à un objet POCO</span><span class="sxs-lookup"><span data-stu-id="eadd6-197">Map secrets to a POCO</span></span>

<span data-ttu-id="eadd6-198">Mappage d’un littéral d’objet entier à un objet POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés connexes.</span><span class="sxs-lookup"><span data-stu-id="eadd6-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eadd6-199">Pour mapper les secrets précédentes à un objet POCO, utilisez le `Configuration` API [de liaison du graphique d’objet](xref:fundamentals/configuration/index#bind-to-an-object-graph) fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="eadd6-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="eadd6-200">Le code suivant lie un personnalisé `MovieSettings` POCO et accède à la `ServiceApiKey` valeur de propriété :</span><span class="sxs-lookup"><span data-stu-id="eadd6-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="eadd6-201">Le `Movies:ConnectionString` et `Movies:ServiceApiKey` secrets sont mappées aux propriétés respectives dans `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="eadd6-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="eadd6-202">Remplacement de chaîne avec des secrets</span><span class="sxs-lookup"><span data-stu-id="eadd6-202">String replacement with secrets</span></span>

<span data-ttu-id="eadd6-203">Stocker les mots de passe en texte brut n’est pas sécurisé.</span><span class="sxs-lookup"><span data-stu-id="eadd6-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="eadd6-204">Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :</span><span class="sxs-lookup"><span data-stu-id="eadd6-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="eadd6-205">Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret.</span><span class="sxs-lookup"><span data-stu-id="eadd6-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="eadd6-206">Exemple :</span><span class="sxs-lookup"><span data-stu-id="eadd6-206">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="eadd6-207">Supprimer le `Password` paire clé-valeur à partir de la chaîne de connexion dans *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="eadd6-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="eadd6-208">Exemple :</span><span class="sxs-lookup"><span data-stu-id="eadd6-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="eadd6-209">Valeur du secret peut être définie sur une <xref:System.Data.SqlClient.SqlConnectionStringBuilder> l’objet <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> propriété pour terminer la chaîne de connexion :</span><span class="sxs-lookup"><span data-stu-id="eadd6-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="eadd6-210">Répertorier les clés secrètes</span><span class="sxs-lookup"><span data-stu-id="eadd6-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eadd6-211">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="eadd6-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="eadd6-212">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="eadd6-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="eadd6-213">Dans l’exemple précédent, un signe deux-points dans les noms de clé désigne la hiérarchie d’objets au sein de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="eadd6-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="eadd6-214">Supprimer un secret unique</span><span class="sxs-lookup"><span data-stu-id="eadd6-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eadd6-215">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="eadd6-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="eadd6-216">L’application *secrets.json* fichier a été modifié pour supprimer la paire clé-valeur associée à la `MoviesConnectionString` clé :</span><span class="sxs-lookup"><span data-stu-id="eadd6-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="eadd6-217">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="eadd6-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="eadd6-218">Supprimer tous les secrets</span><span class="sxs-lookup"><span data-stu-id="eadd6-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eadd6-219">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="eadd6-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="eadd6-220">Tous les secrets d’utilisateur pour l’application ont été supprimés de la *secrets.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="eadd6-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="eadd6-221">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="eadd6-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="eadd6-222">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="eadd6-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
