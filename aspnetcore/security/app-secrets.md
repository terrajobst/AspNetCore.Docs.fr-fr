---
title: Stockage sécurisé des secrets d’application en cours de développement dans ASP.NET Core
author: rick-anderson
description: Montre comment stocker des clés secrètes en toute sécurité pendant le développement
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 166111696a9c4244ede44fca8878dd3725bb3099
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="66715-103">Stockage sécurisé des secrets d’application en cours de développement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66715-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="66715-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="66715-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="66715-105">Ce document montre comment vous pouvez utiliser l’outil Secret Manager dans le développement et comment conserver les clés secrètes en dehors de votre code. </span><span class="sxs-lookup"><span data-stu-id="66715-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="66715-106">Le point le plus important est que vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et vous ne devez pas utiliser les clés secrètes de production en mode de développement et de test.</span><span class="sxs-lookup"><span data-stu-id="66715-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="66715-107">Vous pouvez utiliser à la place la [configuration du système](xref:fundamentals/configuration/index) et lire ces valeurs à partir de variables d’environnement ou à partir de valeurs stockées à l’aide de l'outil Secret Manager. </span><span class="sxs-lookup"><span data-stu-id="66715-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="66715-108">L’outil Secret Manager empêche les données sensibles d'être archivées dans le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="66715-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="66715-109">La [configuration du système](xref:fundamentals/configuration/index) peut lire les clés secrètes stockées avec l’outil Secret Manager décrit dans cet article.</span><span class="sxs-lookup"><span data-stu-id="66715-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="66715-110">L’outil Secret Manager est utilisé uniquement dans le développement.</span><span class="sxs-lookup"><span data-stu-id="66715-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="66715-111">Vous pouvez protéger des secrets de test et de production Azure avec le fournisseur de configuration [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="66715-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="66715-112">Consultez [fournisseur de configuration d’Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="66715-112">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="66715-113">Variables d’environnement</span><span class="sxs-lookup"><span data-stu-id="66715-113">Environment variables</span></span>

<span data-ttu-id="66715-114">Vous pouvez ensuite utiliser des variables d’environnement pour remplacer des valeurs de configuration pour toutes les sources de configuration spécifiées précédemment.</span><span class="sxs-lookup"><span data-stu-id="66715-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="66715-115">Vous pouvez installer la [configuration du framework](xref:fundamentals/configuration/index) pour lire les valeurs de variables d’environnement en utilisant la méthode d'extension `AddEnvironmentVariables` dans la configuration.</span><span class="sxs-lookup"><span data-stu-id="66715-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="66715-116">Vous pouvez ensuite utiliser des variables d’environnement pour remplacer des valeurs de configuration pour toutes les sources de configuration spécifié précédemment.</span><span class="sxs-lookup"><span data-stu-id="66715-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="66715-117">Par exemple, si vous créez une application web ASP.NET Core avec les comptes d’utilisateur individuels, ça ajoutera une chaîne de connexion par défaut pour le fichier de configuration *appsettings.json* du projet avec la clé `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="66715-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="66715-118">La chaîne de connexion par défaut est configurée pour utiliser la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="66715-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="66715-119">Lorsque vous déployez votre application sur un serveur de test ou de production, vous pouvez remplacer la valeur de la clé `DefaultConnection` avec un paramètre de variable d’environnement qui contient la chaîne de connexion (avec éventuellement des informations d’identification sensibles) pour une base de données de test ou de production.</span><span class="sxs-lookup"><span data-stu-id="66715-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="66715-120">Les variables d’environnement sont généralement stockées en texte brut et ne sont pas chiffrées.</span><span class="sxs-lookup"><span data-stu-id="66715-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="66715-121">Donc si l’ordinateur ou le processus est compromis, et les variables d’environnement deviennent accessibles par des tiers non approuvés.</span><span class="sxs-lookup"><span data-stu-id="66715-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="66715-122">Des mesures supplémentaires pour empêcher la divulgation de secrets de l’utilisateur peuvent toujours être nécessaires.</span><span class="sxs-lookup"><span data-stu-id="66715-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="66715-123">L'outil Secret Manager (Gestionnaire de secrets)</span><span class="sxs-lookup"><span data-stu-id="66715-123">Secret Manager</span></span>

<span data-ttu-id="66715-124">L’outil principal Secret Manager stocke des données sensibles pour les travaux de développement en dehors de l’arborescence de votre projet.</span><span class="sxs-lookup"><span data-stu-id="66715-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="66715-125">L’outil Gestionnaire de la clé secrète est un outil de projet qui peut être utilisé pour stocker des secrets pour un projet .NET Core pendant le développement.</span><span class="sxs-lookup"><span data-stu-id="66715-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="66715-126">Avec l’outil Secret Manager, vous pouvez associer des secrets de l’application à un projet spécifique et les partager entre plusieurs projets.</span><span class="sxs-lookup"><span data-stu-id="66715-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="66715-127">L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé.</span><span class="sxs-lookup"><span data-stu-id="66715-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="66715-128">Il est utile uniquement à des fins de développement.</span><span class="sxs-lookup"><span data-stu-id="66715-128">It's for development purposes only.</span></span> <span data-ttu-id="66715-129">Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="66715-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="66715-130">Installation de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="66715-130">Installing the Secret Manager tool</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="66715-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66715-131">Visual Studio</span></span>](#tab/visual-studio/)
<span data-ttu-id="66715-132">Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez dans le menu contextuel **Modifier \<project_name\>.csproj** .</span><span class="sxs-lookup"><span data-stu-id="66715-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="66715-133">Ajoutez la ligne en surbrillance dans le fichier *.csproj* et enregistrez pour restaurer le package NuGet associé :</span><span class="sxs-lookup"><span data-stu-id="66715-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="66715-134">Cliquez à nouveau sur le projet dans l’Explorateur de solutions, puis sélectionnez **Gérer les secrets utilisateur** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="66715-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="66715-135">Cette opération ajoute un nouveau nœud `UserSecretsId` au sein d’un `PropertyGroup` du fichier *.csproj*, comme illustré dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="66715-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="66715-136">L’enregistrement du fichier *.csproj* modifié ouvre également un fichier `secrets.json` dans l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="66715-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="66715-137">Remplacez le contenu du fichier `secrets.json` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="66715-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="66715-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="66715-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)
<span data-ttu-id="66715-139">Ajoutez la référence à `Microsoft.Extensions.SecretManager.Tools` dans le fichier *.csproj* et exécutez la commande [dotnet restauration](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="66715-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="66715-140">Vous pouvez utiliser les mêmes étapes pour installer Secret Manager à l’aide de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="66715-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="66715-141">Pour tester et obtenir l'aide de l’outil Secret Manager exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="66715-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="66715-142">L’outil Secret Manager affiche l’utilisation, les options et l’aide de la commande.</span><span class="sxs-lookup"><span data-stu-id="66715-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="66715-143">Vous devez être dans le même répertoire que le fichier *.csproj* pour exécuter les outils définis dans le fichier *.csproj* du nœud `DotNetCliToolReference`.</span><span class="sxs-lookup"><span data-stu-id="66715-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="66715-144">L’outil Secret Manager opère sur les paramètres de configuration de projet spécifiques qui sont stockés dans votre profil utilisateur.</span><span class="sxs-lookup"><span data-stu-id="66715-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="66715-145">Pour utiliser les secrets de l’utilisateur, le projet doit spécifier la valeur de `UserSecretsId` dans son fichier *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="66715-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="66715-146">La valeur de `UserSecretsId` est arbitraire, mais est généralement unique au projet.</span><span class="sxs-lookup"><span data-stu-id="66715-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="66715-147">Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="66715-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="66715-148">Ajoutez un `UserSecretsId` pour votre projet dans le fichier *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="66715-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="66715-149">Utilisez l’outil Secret Manager pour définir une clé secrète.</span><span class="sxs-lookup"><span data-stu-id="66715-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="66715-150">Par exemple, dans une fenêtre de commande à partir du répertoire de projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="66715-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="66715-151">Vous pouvez exécuter l’outil Secret Manager à partir d’autres annuaires, mais vous devez utiliser l'option `--project` à passer dans le chemin d’accès au fichier *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="66715-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="66715-152">Vous pouvez également utiliser l’outil Secret Manager pour répertorier, supprimer et effacer les secrets de l’application.</span><span class="sxs-lookup"><span data-stu-id="66715-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

* * *
## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="66715-153">L’accès à des secrets de l’utilisateur via la configuration</span><span class="sxs-lookup"><span data-stu-id="66715-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="66715-154">Vous pouvez accéder aux secrets stockés dans Secret Manager via le système de configuration.</span><span class="sxs-lookup"><span data-stu-id="66715-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="66715-155">Ajoutez le package `Microsoft.Extensions.Configuration.UserSecrets` et exécutez la commande [dotnet restore](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="66715-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="66715-156">Ajouter la source de configuration des secrets utilisateurs pour la méthode `Startup`:</span><span class="sxs-lookup"><span data-stu-id="66715-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="66715-157">Vous pouvez accéder à des secrets de l’utilisateur via l’API de configuration :</span><span class="sxs-lookup"><span data-stu-id="66715-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="66715-158">Fonctionnement de l’outil Secret Manager</span><span class="sxs-lookup"><span data-stu-id="66715-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="66715-159">L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées.</span><span class="sxs-lookup"><span data-stu-id="66715-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="66715-160">Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation.</span><span class="sxs-lookup"><span data-stu-id="66715-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="66715-161">Dans la version actuelle, les valeurs sont stockées dans un fichier [JSON](http://json.org/) de configuration dans le répertoire de profil utilisateur :</span><span class="sxs-lookup"><span data-stu-id="66715-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="66715-162">Windows : `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="66715-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="66715-163">Linux : `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="66715-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="66715-164">macOS : `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="66715-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="66715-165">La valeur de `userSecretsId` provient de la valeur spécifiée dans *.csproj* fichier.</span><span class="sxs-lookup"><span data-stu-id="66715-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="66715-166">Vous ne devez pas écrire du code qui dépend de l’emplacement ou du format des données enregistrées avec l’outil Secret Manager, car ces détails d’implémentation peuvent changer.</span><span class="sxs-lookup"><span data-stu-id="66715-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="66715-167">Par exemple, les valeurs de clé secrètes ne sont actuellement *pas* chiffrés, mais le seront peut être un jour.</span><span class="sxs-lookup"><span data-stu-id="66715-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66715-168">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="66715-168">Additional resources</span></span>

* [<span data-ttu-id="66715-169">Configuration</span><span class="sxs-lookup"><span data-stu-id="66715-169">Configuration</span></span>](xref:fundamentals/configuration/index)
