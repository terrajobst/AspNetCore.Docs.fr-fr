---
title: Stockage sécurisé des secrets d’application en développement dans ASP.NET Core
author: rick-anderson
description: Découvrez comment stocker et récupérer des informations sensibles en tant que secrets d’application lors du développement d’une application ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 9b36ae64fbe277cd81ed22ba7b21b0a035082dbd
ms.sourcegitcommit: c815a9465e7b1bab44ce1643ec345b33e6cf1598
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606790"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="11eaa-103">Stockage sécurisé des secrets d’application en développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11eaa-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="11eaa-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="11eaa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="11eaa-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11eaa-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="11eaa-106">Ce document décrit les techniques de stockage et de récupération des données sensibles lors du développement d’une application ASP.NET Core sur un ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="11eaa-106">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="11eaa-107">Ne stockez jamais de mots de passe ou d’autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="11eaa-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="11eaa-108">Les secrets de production ne doivent pas être utilisés à des fins de développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="11eaa-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="11eaa-109">Les secrets ne doivent pas être déployés avec l’application.</span><span class="sxs-lookup"><span data-stu-id="11eaa-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="11eaa-110">Au lieu de cela, les secrets doivent être mis à disposition dans l’environnement de production par un moyen contrôlé, comme les variables d’environnement, les Azure Key Vault, etc. Vous pouvez stocker et protéger les secrets de test et de production Azure à l’aide du [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="11eaa-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="11eaa-111">Variables d'environnement</span><span class="sxs-lookup"><span data-stu-id="11eaa-111">Environment variables</span></span>

<span data-ttu-id="11eaa-112">Les variables d’environnement sont utilisées pour éviter le stockage des secrets d’application dans le code ou dans des fichiers de configuration locaux.</span><span class="sxs-lookup"><span data-stu-id="11eaa-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="11eaa-113">Les variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.</span><span class="sxs-lookup"><span data-stu-id="11eaa-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="11eaa-114">Configurez la lecture des valeurs de variable d’environnement en appelant <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> dans le constructeur `Startup` :</span><span class="sxs-lookup"><span data-stu-id="11eaa-114">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="11eaa-115">Prenons l’exemple d’une application Web ASP.NET Core dans laquelle la sécurité **des comptes d’utilisateur individuels** est activée.</span><span class="sxs-lookup"><span data-stu-id="11eaa-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="11eaa-116">Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appSettings. JSON* du projet avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="11eaa-117">La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="11eaa-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="11eaa-118">Pendant le déploiement de l’application, la valeur de clé de `DefaultConnection` peut être remplacée par la valeur d’une variable d’environnement.</span><span class="sxs-lookup"><span data-stu-id="11eaa-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="11eaa-119">La variable d’environnement peut stocker la chaîne de connexion complète avec des informations d’identification sensibles.</span><span class="sxs-lookup"><span data-stu-id="11eaa-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="11eaa-120">Les variables d’environnement sont généralement stockées en texte brut et non chiffré.</span><span class="sxs-lookup"><span data-stu-id="11eaa-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="11eaa-121">Si l’ordinateur ou le processus est compromis, les variables d’environnement sont accessibles aux parties non fiables.</span><span class="sxs-lookup"><span data-stu-id="11eaa-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="11eaa-122">Des mesures supplémentaires pour empêcher la divulgation de secrets d’utilisateur peuvent être nécessaires.</span><span class="sxs-lookup"><span data-stu-id="11eaa-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="11eaa-123">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="11eaa-123">Secret Manager</span></span>

<span data-ttu-id="11eaa-124">L’outil secret Manager stocke les données sensibles pendant le développement d’un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="11eaa-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="11eaa-125">Dans ce contexte, un élément de données sensibles est un secret d’application.</span><span class="sxs-lookup"><span data-stu-id="11eaa-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="11eaa-126">Les secrets de l’application sont stockés dans un emplacement distinct de l’arborescence du projet.</span><span class="sxs-lookup"><span data-stu-id="11eaa-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="11eaa-127">Les secrets de l’application sont associés à un projet spécifique ou partagés entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="11eaa-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="11eaa-128">Les secrets de l’application ne sont pas archivés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="11eaa-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="11eaa-129">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="11eaa-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="11eaa-130">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="11eaa-130">It's for development purposes only.</span></span> <span data-ttu-id="11eaa-131">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="11eaa-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="11eaa-132">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="11eaa-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="11eaa-133">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="11eaa-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="11eaa-134">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="11eaa-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="11eaa-135">Les valeurs sont stockées dans un fichier de configuration JSON dans un dossier de profil utilisateur protégé du système sur l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="11eaa-135">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="11eaa-136">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="11eaa-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="11eaa-137">Chemin d’accès au système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="11eaa-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="11eaa-138">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="11eaa-138">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="11eaa-139">Chemin d’accès au système de fichiers :</span><span class="sxs-lookup"><span data-stu-id="11eaa-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="11eaa-140">Dans les chemins d’accès de fichier précédents, remplacez `<user_secrets_id>` par la valeur `UserSecretsId` spécifiée dans le fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="11eaa-141">N’écrivez pas de code dépendant de l’emplacement ou du format des données enregistrées avec l’outil secret Manager.</span><span class="sxs-lookup"><span data-stu-id="11eaa-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="11eaa-142">Ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="11eaa-142">These implementation details may change.</span></span> <span data-ttu-id="11eaa-143">Par exemple, les valeurs secrètes ne sont pas chiffrées, mais peuvent être dans le futur.</span><span class="sxs-lookup"><span data-stu-id="11eaa-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="11eaa-144">Installer l’outil secret Manager</span><span class="sxs-lookup"><span data-stu-id="11eaa-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="11eaa-145">L’outil Gestionnaire de secret est fourni avec le CLI .NET Core dans kit SDK .NET Core 2.1.300 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="11eaa-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="11eaa-146">Pour les versions kit SDK .NET Core antérieures à 2.1.300, l’installation des outils est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="11eaa-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="11eaa-147">Exécutez `dotnet --version` à partir d’une interface de commande pour voir le numéro de version du kit SDK .NET Core installé.</span><span class="sxs-lookup"><span data-stu-id="11eaa-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="11eaa-148">Un avertissement s’affiche si le kit SDK .NET Core utilisé comprend l’outil :</span><span class="sxs-lookup"><span data-stu-id="11eaa-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="11eaa-149">Installez le package NuGet [Microsoft. extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) dans votre projet ASP.net core.</span><span class="sxs-lookup"><span data-stu-id="11eaa-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="11eaa-150">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="11eaa-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="11eaa-151">Exécutez la commande suivante dans une interface de commande pour valider l’installation de l’outil :</span><span class="sxs-lookup"><span data-stu-id="11eaa-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="11eaa-152">L’outil Gestionnaire de secret affiche des exemples d’utilisation, d’options et d’aide sur les commandes :</span><span class="sxs-lookup"><span data-stu-id="11eaa-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="11eaa-153">Vous devez être dans le même répertoire que le fichier *. csproj* pour exécuter les outils définis dans les éléments `DotNetCliToolReference` du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="11eaa-154">Activer le stockage secret</span><span class="sxs-lookup"><span data-stu-id="11eaa-154">Enable secret storage</span></span>

<span data-ttu-id="11eaa-155">L’outil Gestionnaire de secret fonctionne sur les paramètres de configuration spécifiques au projet stockés dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="11eaa-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="11eaa-156">L’outil Gestionnaire de secret comprend une commande `init` dans kit SDK .NET Core 3.0.100 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="11eaa-156">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="11eaa-157">Pour utiliser des secrets d’utilisateur, exécutez la commande suivante dans le répertoire du projet :</span><span class="sxs-lookup"><span data-stu-id="11eaa-157">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="11eaa-158">La commande précédente ajoute un élément `UserSecretsId` dans une `PropertyGroup` du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-158">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="11eaa-159">Par défaut, le texte interne de `UserSecretsId` est un GUID.</span><span class="sxs-lookup"><span data-stu-id="11eaa-159">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="11eaa-160">Le texte interne est arbitraire, mais il est unique pour le projet.</span><span class="sxs-lookup"><span data-stu-id="11eaa-160">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="11eaa-161">Pour utiliser des secrets d’utilisateur, définissez un élément `UserSecretsId` dans un `PropertyGroup` du fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-161">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="11eaa-162">Le texte interne de `UserSecretsId` est arbitraire, mais il est unique pour le projet.</span><span class="sxs-lookup"><span data-stu-id="11eaa-162">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="11eaa-163">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-163">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="11eaa-164">Dans Visual Studio, cliquez avec le bouton droit sur le projet dans Explorateur de solutions, puis sélectionnez **gérer les secrets d’utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="11eaa-164">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="11eaa-165">Ce geste ajoute un élément `UserSecretsId`, rempli avec un GUID, au fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-165">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="11eaa-166">Définir une clé secrète</span><span class="sxs-lookup"><span data-stu-id="11eaa-166">Set a secret</span></span>

<span data-ttu-id="11eaa-167">Définissez un secret d’application comprenant une clé et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="11eaa-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="11eaa-168">Le secret est associé à la valeur de `UserSecretsId` du projet.</span><span class="sxs-lookup"><span data-stu-id="11eaa-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="11eaa-169">Par exemple, exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="11eaa-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="11eaa-170">Dans l’exemple précédent, le signe deux-points indique que `Movies` est un littéral d’objet avec une propriété `ServiceApiKey`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="11eaa-171">L’outil Gestionnaire de secret peut également être utilisé à partir d’autres annuaires.</span><span class="sxs-lookup"><span data-stu-id="11eaa-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="11eaa-172">Utilisez l’option `--project` pour fournir le chemin d’accès au système de fichiers où se trouve le fichier *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="11eaa-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="11eaa-173">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="11eaa-173">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="11eaa-174">Aplatissement de la structure JSON dans Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11eaa-174">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="11eaa-175">Le geste **gérer les secrets** de l’utilisateur de Visual Studio ouvre un fichier *secrets. JSON* dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="11eaa-175">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="11eaa-176">Remplacez le contenu de *secrets. JSON* par les paires clé-valeur à stocker.</span><span class="sxs-lookup"><span data-stu-id="11eaa-176">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="11eaa-177">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="11eaa-177">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="11eaa-178">La structure JSON est aplatie après des modifications via `dotnet user-secrets remove` ou `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-178">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="11eaa-179">Par exemple, l’exécution de `dotnet user-secrets remove "Movies:ConnectionString"` réduit le littéral d’objet `Movies`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-179">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="11eaa-180">Le fichier modifié ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="11eaa-180">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="11eaa-181">Définir plusieurs secrets</span><span class="sxs-lookup"><span data-stu-id="11eaa-181">Set multiple secrets</span></span>

<span data-ttu-id="11eaa-182">Un lot de secrets peut être défini par la canalisation JSON à la commande `set`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-182">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="11eaa-183">Dans l’exemple suivant, le contenu du fichier *Input. JSON* est dirigé vers la commande `set`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-183">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="11eaa-184">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="11eaa-184">Windows</span></span>](#tab/windows)

<span data-ttu-id="11eaa-185">Ouvrez une interface de commande, puis exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="11eaa-185">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="11eaa-186">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="11eaa-186">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="11eaa-187">Ouvrez une interface de commande, puis exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="11eaa-187">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="11eaa-188">Accéder à une clé secrète</span><span class="sxs-lookup"><span data-stu-id="11eaa-188">Access a secret</span></span>

<span data-ttu-id="11eaa-189">L' [API de Configuration ASP.net Core](xref:fundamentals/configuration/index) permet d’accéder aux secrets du gestionnaire de secret.</span><span class="sxs-lookup"><span data-stu-id="11eaa-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="11eaa-190">Si votre projet cible .NET Framework, installez le package NuGet [Microsoft. extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="11eaa-190">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11eaa-191">Dans ASP.NET Core 2,0 ou version ultérieure, la source de configuration des secrets de l’utilisateur est automatiquement ajoutée en mode de développement lorsque le projet appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> pour initialiser une nouvelle instance de l’hôte avec des valeurs par défaut préconfigurées.</span><span class="sxs-lookup"><span data-stu-id="11eaa-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="11eaa-192">`CreateDefaultBuilder` appelle <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> lorsque le <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> est <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="11eaa-192">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="11eaa-193">Quand `CreateDefaultBuilder` n’est pas appelé, ajoutez explicitement la source de configuration des secrets de l’utilisateur en appelant <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> dans le constructeur `Startup`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-193">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="11eaa-194">Appelez `AddUserSecrets` uniquement lorsque l’application s’exécute dans l’environnement de développement, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="11eaa-194">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="11eaa-195">Installez le package NuGet [Microsoft. extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="11eaa-195">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="11eaa-196">Ajoutez la source de configuration des secrets de l’utilisateur à l’aide d’un appel à <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> dans le constructeur `Startup` :</span><span class="sxs-lookup"><span data-stu-id="11eaa-196">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="11eaa-197">Les secrets de l’utilisateur peuvent être récupérés via l’API `Configuration` :</span><span class="sxs-lookup"><span data-stu-id="11eaa-197">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="11eaa-198">Mapper les secrets à un POCO</span><span class="sxs-lookup"><span data-stu-id="11eaa-198">Map secrets to a POCO</span></span>

<span data-ttu-id="11eaa-199">Le mappage d’un littéral d’objet entier à un POCO (une classe .NET simple avec des propriétés) est utile pour l’agrégation des propriétés associées.</span><span class="sxs-lookup"><span data-stu-id="11eaa-199">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="11eaa-200">Pour mapper les secrets précédents à un POCO, utilisez la fonctionnalité de [liaison de graphique d’objets](xref:fundamentals/configuration/index#bind-to-an-object-graph) de l’API `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="11eaa-200">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="11eaa-201">Le code suivant est lié à un `MovieSettings` POCO personnalisé et accède à la valeur de la propriété `ServiceApiKey` :</span><span class="sxs-lookup"><span data-stu-id="11eaa-201">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="11eaa-202">Les secrets `Movies:ConnectionString` et `Movies:ServiceApiKey` sont mappés aux propriétés respectives dans `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="11eaa-202">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="11eaa-203">Remplacement de chaîne par des secrets</span><span class="sxs-lookup"><span data-stu-id="11eaa-203">String replacement with secrets</span></span>

<span data-ttu-id="11eaa-204">Le stockage des mots de passe en texte brut n’est pas sécurisé.</span><span class="sxs-lookup"><span data-stu-id="11eaa-204">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="11eaa-205">Par exemple, une chaîne de connexion de base de données stockée dans *appSettings. JSON* peut inclure un mot de passe pour l’utilisateur spécifié :</span><span class="sxs-lookup"><span data-stu-id="11eaa-205">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="11eaa-206">Une approche plus sécurisée consiste à stocker le mot de passe en tant que secret.</span><span class="sxs-lookup"><span data-stu-id="11eaa-206">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="11eaa-207">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="11eaa-207">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="11eaa-208">Supprimez la paire clé-valeur `Password` de la chaîne de connexion dans *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="11eaa-208">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="11eaa-209">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="11eaa-209">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="11eaa-210">La valeur du secret peut être définie sur la propriété <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> d’un objet <xref:System.Data.SqlClient.SqlConnectionStringBuilder> pour terminer la chaîne de connexion :</span><span class="sxs-lookup"><span data-stu-id="11eaa-210">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="11eaa-211">Répertorier les secrets</span><span class="sxs-lookup"><span data-stu-id="11eaa-211">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="11eaa-212">Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="11eaa-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="11eaa-213">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="11eaa-213">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="11eaa-214">Dans l’exemple précédent, un signe deux-points dans le nom de clé désigne la hiérarchie d’objets dans *secrets. JSON*.</span><span class="sxs-lookup"><span data-stu-id="11eaa-214">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="11eaa-215">Supprimer une seule clé secrète</span><span class="sxs-lookup"><span data-stu-id="11eaa-215">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="11eaa-216">Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="11eaa-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="11eaa-217">Le fichier *secrets. JSON* de l’application a été modifié pour supprimer la paire clé-valeur associée à la clé de `MoviesConnectionString` :</span><span class="sxs-lookup"><span data-stu-id="11eaa-217">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="11eaa-218">L’exécution de `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="11eaa-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="11eaa-219">Supprimer tous les secrets</span><span class="sxs-lookup"><span data-stu-id="11eaa-219">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="11eaa-220">Exécutez la commande suivante à partir du répertoire dans lequel se trouve le fichier *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="11eaa-220">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="11eaa-221">Tous les secrets d’utilisateur de l’application ont été supprimés du fichier *secrets. JSON* :</span><span class="sxs-lookup"><span data-stu-id="11eaa-221">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="11eaa-222">L’exécution de `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="11eaa-222">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="11eaa-223">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="11eaa-223">Additional resources</span></span>

* <span data-ttu-id="11eaa-224">Pour plus d’informations sur l’accès au gestionnaire de secret à partir d’IIS, consultez [ce problème](https://github.com/aspnet/AspNetCore.Docs/issues/16328) .</span><span class="sxs-lookup"><span data-stu-id="11eaa-224">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
