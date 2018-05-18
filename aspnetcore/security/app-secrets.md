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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="d9973-103">Stockage sécurisé des secrets d’application en cours de développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9973-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="d9973-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), et [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d9973-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9973-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="d9973-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9973-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="d9973-107">Ce document explique les techniques pour stocker et récupérer des données sensibles pendant le développement d’une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9973-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="d9973-108">Vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et ne doivent pas utiliser les secrets de fabrication dans le développement ou le mode.</span><span class="sxs-lookup"><span data-stu-id="d9973-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="d9973-109">Vous pouvez stocker et protéger des secrets de test et de production Azure avec la [fournisseur de configuration d’Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="d9973-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="d9973-110">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="d9973-110">Environment variables</span></span>

<span data-ttu-id="d9973-111">Variables d’environnement sont utilisés pour éviter le stockage de secrets d’application dans le code ou dans les fichiers de configuration local.</span><span class="sxs-lookup"><span data-stu-id="d9973-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="d9973-112">Variables d’environnement remplacent les valeurs de configuration pour toutes les sources de configuration spécifié précédemment.</span><span class="sxs-lookup"><span data-stu-id="d9973-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-113">Configurer la lecture des valeurs de variable d’environnement en appelant [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) dans le `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="d9973-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="d9973-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="d9973-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="d9973-115">Imaginez une application de web ASP.NET Core dans lequel **comptes d’utilisateur individuels** la sécurité est activée.</span><span class="sxs-lookup"><span data-stu-id="d9973-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="d9973-116">Une chaîne de connexion de base de données par défaut est incluse dans le fichier *appsettings.json* fichier avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="d9973-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="d9973-117">La chaîne de connexion par défaut est pour la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d9973-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="d9973-118">Pendant le déploiement de l’application, le `DefaultConnection` valeur de clé peut être remplacée par la valeur d’une variable environnement.</span><span class="sxs-lookup"><span data-stu-id="d9973-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="d9973-119">La variable d’environnement peut stocker la chaîne de connexion complète avec les informations d’identification sensibles.</span><span class="sxs-lookup"><span data-stu-id="d9973-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="d9973-120">Variables d’environnement sont généralement stockés en texte brut et non chiffré.</span><span class="sxs-lookup"><span data-stu-id="d9973-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="d9973-121">Si l’ordinateur ou le processus est compromis, variables d’environnement accessibles par des tiers non approuvés.</span><span class="sxs-lookup"><span data-stu-id="d9973-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="d9973-122">Des mesures supplémentaires pour empêcher la divulgation de secrets de l’utilisateur peuvent être nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d9973-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="d9973-123">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="d9973-123">Secret Manager</span></span>

<span data-ttu-id="d9973-124">L’outil Gestionnaire de secret principal stocke des données sensibles pendant le développement d’un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d9973-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="d9973-125">Dans ce contexte, une partie des données sensibles est un secret d’application.</span><span class="sxs-lookup"><span data-stu-id="d9973-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="d9973-126">Secrets d’application sont stockés dans un emplacement distinct à partir de l’arborescence du projet.</span><span class="sxs-lookup"><span data-stu-id="d9973-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="d9973-127">Les secrets de l’application sont associés à un projet spécifique ou partagés entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="d9973-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="d9973-128">Les secrets de l’application ne sont pas archivés dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="d9973-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="d9973-129">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="d9973-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="d9973-130">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="d9973-130">It's for development purposes only.</span></span> <span data-ttu-id="d9973-131">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d9973-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="d9973-132">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="d9973-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="d9973-133">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="d9973-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="d9973-134">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="d9973-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="d9973-135">Les valeurs sont stockées dans un [JSON](https://json.org/) fichier de configuration dans un dossier de profil utilisateur protégés par le système sur l’ordinateur local :</span><span class="sxs-lookup"><span data-stu-id="d9973-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="d9973-136">Windows : `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="d9973-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="d9973-137">Linux et macOS : `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="d9973-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="d9973-138">Dans l’exemple précédent, les chemins de fichiers, remplacez `<user_secrets_id>` avec la `UserSecretsId` valeur spécifiée dans le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="d9973-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="d9973-139">Ne pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Gestionnaire de Secret.</span><span class="sxs-lookup"><span data-stu-id="d9973-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="d9973-140">Ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="d9973-140">These implementation details may change.</span></span> <span data-ttu-id="d9973-141">Par exemple, les valeurs de secret principal ne sont pas chiffrées, mais peut être à l’avenir.</span><span class="sxs-lookup"><span data-stu-id="d9973-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="d9973-142">Installer le Gestionnaire de la clé secrète</span><span class="sxs-lookup"><span data-stu-id="d9973-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="d9973-143">L’outil Gestionnaire de la clé secrète est fourni avec l’interface de ligne de base .NET dans .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="d9973-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="d9973-144">.NET Core SDK 2.0 ou version antérieure, l’installation de l’outil est nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d9973-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="d9973-145">Installer le [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) package NuGet dans votre projet ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="d9973-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="d9973-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="d9973-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="d9973-147">Exécutez la commande suivante dans une invite de commandes pour valider l’installation de l’outil :</span><span class="sxs-lookup"><span data-stu-id="d9973-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="d9973-148">L’outil Gestionnaire de Secret affiche l’utilisation de l’exemple, options et l’aide de la commande :</span><span class="sxs-lookup"><span data-stu-id="d9973-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="d9973-149">Vous devez être dans le même répertoire que le *.csproj* fichier pour exécuter les outils définis dans le *.csproj* du fichier `DotNetCliToolReference` éléments.</span><span class="sxs-lookup"><span data-stu-id="d9973-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="d9973-150">Définir une clé secrète</span><span class="sxs-lookup"><span data-stu-id="d9973-150">Set a secret</span></span>

<span data-ttu-id="d9973-151">L’outil Gestionnaire de Secret opère sur les paramètres de configuration de projet spécifiques stockées dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d9973-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="d9973-152">Pour utiliser les secrets des utilisateurs, définir un `UserSecretsId` élément au sein d’un `PropertyGroup` de la *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="d9973-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="d9973-153">La valeur de `UserSecretsId` est arbitraire, mais est unique au projet.</span><span class="sxs-lookup"><span data-stu-id="d9973-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="d9973-154">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="d9973-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="d9973-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="d9973-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="d9973-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="d9973-157">Dans Visual Studio, cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="d9973-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="d9973-158">Cette opération ajoute une `UserSecretsId` élément, rempli avec un GUID, à le *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="d9973-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="d9973-159">Visual Studio ouvre un *secrets.json* fichier dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="d9973-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="d9973-160">Remplacez le contenu de *secrets.json* avec les paires clé-valeur à stocker.</span><span class="sxs-lookup"><span data-stu-id="d9973-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="d9973-161">Par exemple :[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="d9973-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="d9973-162">Définir un secret d’application comprenant une clé et sa valeur.</span><span class="sxs-lookup"><span data-stu-id="d9973-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="d9973-163">Le secret n’est associé au projet `UserSecretsId` valeur.</span><span class="sxs-lookup"><span data-stu-id="d9973-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="d9973-164">Par exemple, exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="d9973-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="d9973-165">Dans l’exemple précédent, le signe deux-points indique que `Movies` est un objet littéral avec un `ServiceApiKey` propriété.</span><span class="sxs-lookup"><span data-stu-id="d9973-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="d9973-166">L’outil Gestionnaire de la clé secrète peut servir à partir d’autres annuaires trop.</span><span class="sxs-lookup"><span data-stu-id="d9973-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="d9973-167">Utilisez le `--project` option pour indiquer le chemin d’accès de système de fichiers auquel le *.csproj* fichier existe.</span><span class="sxs-lookup"><span data-stu-id="d9973-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="d9973-168">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d9973-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="d9973-169">Définir plusieurs clés secrètes</span><span class="sxs-lookup"><span data-stu-id="d9973-169">Set multiple secrets</span></span>

<span data-ttu-id="d9973-170">Un lot de secrets peut être défini en transférant JSON à le `set` commande.</span><span class="sxs-lookup"><span data-stu-id="d9973-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="d9973-171">Dans l’exemple suivant, la *input.json* contenu du fichier est dirigé vers le `set` commande sur Windows :</span><span class="sxs-lookup"><span data-stu-id="d9973-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="d9973-172">Sur Mac OS et Linux, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d9973-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="d9973-173">Accéder à une clé secrète</span><span class="sxs-lookup"><span data-stu-id="d9973-173">Access a secret</span></span>

<span data-ttu-id="d9973-174">Le [API de Configuration ASP.NET Core](xref:fundamentals/configuration/index) fournit l’accès aux clés secrètes de gestionnaire de Secret.</span><span class="sxs-lookup"><span data-stu-id="d9973-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="d9973-175">Si ciblant .NET Core 1.x ou .NET Framework, installez le [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="d9973-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-176">Ajouter la source de configuration utilisateur secrets pour la `Startup` constructeur :</span><span class="sxs-lookup"><span data-stu-id="d9973-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="d9973-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="d9973-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="d9973-178">Secrets de l’utilisateur peuvent être récupérés via la `Configuration` API :</span><span class="sxs-lookup"><span data-stu-id="d9973-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="d9973-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="d9973-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="d9973-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="d9973-181">Remplacement de chaîne avec les clés secrètes</span><span class="sxs-lookup"><span data-stu-id="d9973-181">String replacement with secrets</span></span>

<span data-ttu-id="d9973-182">Il est risqué de stocker des mots de passe en texte brut.</span><span class="sxs-lookup"><span data-stu-id="d9973-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="d9973-183">Par exemple, une chaîne de connexion de base de données stockées dans *appsettings.json* peut inclure un mot de passe pour l’utilisateur spécifié :</span><span class="sxs-lookup"><span data-stu-id="d9973-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="d9973-184">Une approche plus sécurisée consiste pour stocker le mot de passe en tant que secret.</span><span class="sxs-lookup"><span data-stu-id="d9973-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="d9973-185">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d9973-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="d9973-186">Remplacez le mot de passe *appsettings.json* par un espace réservé.</span><span class="sxs-lookup"><span data-stu-id="d9973-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="d9973-187">Dans l’exemple suivant, `{0}` est utilisé comme espace réservé pour former un [chaîne de Format Composite](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="d9973-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="d9973-188">Valeur de la clé secrète peut être injectée dans l’espace réservé pour terminer la chaîne de connexion :</span><span class="sxs-lookup"><span data-stu-id="d9973-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="d9973-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="d9973-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="d9973-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="d9973-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="d9973-191">Répertorier les clés secrètes</span><span class="sxs-lookup"><span data-stu-id="d9973-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="d9973-192">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="d9973-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="d9973-193">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="d9973-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="d9973-194">Dans l’exemple précédent, un signe deux-points dans les noms de clé indique au sein de la hiérarchie d’objets *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="d9973-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="d9973-195">Supprimer une clé secrète unique</span><span class="sxs-lookup"><span data-stu-id="d9973-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="d9973-196">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="d9973-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="d9973-197">L’application *secrets.json* fichier a été modifié pour supprimer la paire clé-valeur associée à la `MoviesConnectionString` clé :</span><span class="sxs-lookup"><span data-stu-id="d9973-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="d9973-198">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="d9973-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="d9973-199">Supprimez tous les secrets</span><span class="sxs-lookup"><span data-stu-id="d9973-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="d9973-200">Exécutez la commande suivante à partir du répertoire dans lequel le *.csproj* fichier existe :</span><span class="sxs-lookup"><span data-stu-id="d9973-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="d9973-201">Tous les secrets des utilisateurs pour l’application ont été supprimés de la *secrets.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="d9973-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="d9973-202">En cours d’exécution `dotnet user-secrets list` affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="d9973-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="d9973-203">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d9973-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>