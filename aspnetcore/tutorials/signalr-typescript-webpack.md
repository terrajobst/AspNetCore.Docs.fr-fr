---
title: Utiliser ASP.NET Core SignalR avec TypeScript et Webpack
author: ssougnez
description: Dans ce tutoriel, vous configurez Webpack pour regrouper et générer une application web ASP.NET Core SignalR dont le client est écrit en TypeScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7b39bbf657244db83e9d60014a5759000eb5f14
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206950"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="27d0f-103">Utiliser ASP.NET Core SignalR avec TypeScript et Webpack</span><span class="sxs-lookup"><span data-stu-id="27d0f-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="27d0f-104">Par [Sébastien Sougnez](https://twitter.com/ssougnez) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="27d0f-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="27d0f-105">[Webpack](https://webpack.js.org/) permet aux développeurs de regrouper et générer les ressources côté client d’une application web.</span><span class="sxs-lookup"><span data-stu-id="27d0f-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="27d0f-106">Ce tutoriel montre comment utiliser Webpack dans une application web ASP.NET Core SignalR dont le client est écrit en [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="27d0f-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="27d0f-107">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="27d0f-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="27d0f-108">Générer automatiquement un modèle d’application ASP.NET Core SignalR de démarrage</span><span class="sxs-lookup"><span data-stu-id="27d0f-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="27d0f-109">Configurer le client TypeScript SignalR</span><span class="sxs-lookup"><span data-stu-id="27d0f-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="27d0f-110">Configurer un pipeline de build à l’aide de Webpack</span><span class="sxs-lookup"><span data-stu-id="27d0f-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="27d0f-111">Configurer le serveur SignalR</span><span class="sxs-lookup"><span data-stu-id="27d0f-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="27d0f-112">Activer la communication entre le client et le serveur</span><span class="sxs-lookup"><span data-stu-id="27d0f-112">Enable communication between client and server</span></span>

<span data-ttu-id="27d0f-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27d0f-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27d0f-114">Prérequis</span><span class="sxs-lookup"><span data-stu-id="27d0f-114">Prerequisites</span></span>

<span data-ttu-id="27d0f-115">Installez les logiciels suivants :</span><span class="sxs-lookup"><span data-stu-id="27d0f-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27d0f-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27d0f-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="27d0f-117">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="27d0f-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="27d0f-118">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="27d0f-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="27d0f-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 ou ultérieure avec la charge de travail **ASP.NET et développement web**</span><span class="sxs-lookup"><span data-stu-id="27d0f-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="27d0f-120">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="27d0f-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="27d0f-121">Kit SDK .NET Core 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="27d0f-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="27d0f-122">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="27d0f-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="27d0f-123">Créer l’application web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27d0f-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27d0f-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27d0f-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="27d0f-125">Configurez Visual Studio pour rechercher npm dans la variable d'environnement *PATH*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="27d0f-126">Par défaut, Visual Studio utilise la version de npm qu’il trouve dans son répertoire d’installation.</span><span class="sxs-lookup"><span data-stu-id="27d0f-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="27d0f-127">Suivez ces instructions dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="27d0f-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="27d0f-128">Accédez à **Outils** > **Options** > **Projets et solutions** > **Gestion des packages web** > **Outils web externes**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="27d0f-129">Sélectionnez l’entrée *$(PATH)* dans la liste.</span><span class="sxs-lookup"><span data-stu-id="27d0f-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="27d0f-130">Cliquez sur la flèche vers le haut pour déplacer l’entrée à la deuxième position dans la liste.</span><span class="sxs-lookup"><span data-stu-id="27d0f-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="27d0f-131">La première entrée référence les packages locaux du projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Configuration de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="27d0f-133">La configuration de Visual Studio est terminée.</span><span class="sxs-lookup"><span data-stu-id="27d0f-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="27d0f-134">Nous allons maintenant créer le projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-134">It's time to create the project.</span></span>

1. <span data-ttu-id="27d0f-135">Utilisez l’option de menu **Fichier** > **Nouveau** > **Projet** et choisissez le modèle **Application web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="27d0f-136">Nommez le projet *SignalRWebPack*, puis cliquez sur le bouton **OK**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="27d0f-137">Sélectionnez *.NET Core* dans la liste déroulante du framework cible, puis *ASP.NET Core 2.1* dans la liste déroulante du sélecteur de framework.</span><span class="sxs-lookup"><span data-stu-id="27d0f-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="27d0f-138">Sélectionnez le modèle **vide**, puis cliquez sur le bouton **OK**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="27d0f-139">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="27d0f-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="27d0f-140">Exécutez la commande suivante dans le **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="27d0f-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="27d0f-141">Une application web ASP.NET Core vide, ciblant .NET Core, est créée dans un répertoire *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="27d0f-142">Configurer Webpack et TypeScript</span><span class="sxs-lookup"><span data-stu-id="27d0f-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="27d0f-143">Les étapes suivantes configurent la conversion de TypeScript en JavaScript et le regroupement des ressources côté client.</span><span class="sxs-lookup"><span data-stu-id="27d0f-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="27d0f-144">Exécutez la commande suivante à la racine du projet pour créer un fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="27d0f-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="27d0f-145">Ajoutez la propriété en surbrillance au fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="27d0f-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="27d0f-146">La définition de la propriété `private` sur `true` empêche les avertissements d’installation de package à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="27d0f-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="27d0f-147">Installez les packages npm nécessaires.</span><span class="sxs-lookup"><span data-stu-id="27d0f-147">Install the required npm packages.</span></span> <span data-ttu-id="27d0f-148">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="27d0f-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="27d0f-149">Détails de commande à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="27d0f-149">Some command details to note:</span></span>

    * <span data-ttu-id="27d0f-150">Un numéro de version suit le signe `@` pour chaque nom de package.</span><span class="sxs-lookup"><span data-stu-id="27d0f-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="27d0f-151">npm installe ces versions de package spécifiques.</span><span class="sxs-lookup"><span data-stu-id="27d0f-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="27d0f-152">L’option `-E` désactive le comportement par défaut de npm consistant à écrire des opérateurs de plage de [gestion sémantique des versions](https://semver.org/) dans *package.json*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="27d0f-153">Par exemple, `"webpack": "4.12.0"` peut être utilisé à la place de `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="27d0f-154">Cette option empêche les mises à niveau involontaires vers des versions de package plus récentes.</span><span class="sxs-lookup"><span data-stu-id="27d0f-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="27d0f-155">Consultez la documentation officielle de [npm-install](https://docs.npmjs.com/cli/install) pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="27d0f-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="27d0f-156">Remplacez la propriété `scripts` du fichier *package.json* par l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="27d0f-157">Explication des scripts :</span><span class="sxs-lookup"><span data-stu-id="27d0f-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="27d0f-158">`build` : Regroupe vos ressources côté client en mode de développement et surveille les changements de fichier.</span><span class="sxs-lookup"><span data-stu-id="27d0f-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="27d0f-159">L’observateur de fichiers force la regénération du regroupement chaque fois qu’un fichier projet change.</span><span class="sxs-lookup"><span data-stu-id="27d0f-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="27d0f-160">L’option `mode` désactive les optimisations de production, comme la minimisation de l’arborescence (tree shaking).</span><span class="sxs-lookup"><span data-stu-id="27d0f-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="27d0f-161">Utilisez uniquement `build` dans le développement.</span><span class="sxs-lookup"><span data-stu-id="27d0f-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="27d0f-162">`release` : Regroupe vos ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="27d0f-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="27d0f-163">`publish` : Exécute le script `release` pour regrouper les ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="27d0f-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="27d0f-164">La commande appelle la commande [publish](/dotnet/core/tools/dotnet-publish) de CLI .NET Core pour publier l’application.</span><span class="sxs-lookup"><span data-stu-id="27d0f-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="27d0f-165">Créez un fichier nommé *webpack.config.js* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="27d0f-166">Le fichier précédent configure la compilation Webpack.</span><span class="sxs-lookup"><span data-stu-id="27d0f-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="27d0f-167">Détails de configuration à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="27d0f-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="27d0f-168">La propriété `output` remplace la valeur par défaut de *dist*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="27d0f-169">Le regroupement est émis dans le répertoire *wwwroot* à la place.</span><span class="sxs-lookup"><span data-stu-id="27d0f-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="27d0f-170">Le tableau `resolve.extensions` inclut *.js* pour importer le client JavaScript SignalR.</span><span class="sxs-lookup"><span data-stu-id="27d0f-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="27d0f-171">Créez un répertoire *src* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="27d0f-172">Son objectif est de stocker les ressources côté client du projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="27d0f-173">Créez *src/index.html* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="27d0f-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="27d0f-174">Le code HTML précédent définit le balisage réutilisable de la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="27d0f-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="27d0f-175">Créez un répertoire *src/css*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="27d0f-176">Son objectif est de stocker les fichiers *.css* du projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="27d0f-177">Créez *src/css/main.css* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="27d0f-178">Le fichier *main.css* précédent définit le style de l’application.</span><span class="sxs-lookup"><span data-stu-id="27d0f-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="27d0f-179">Créez *src/tsconfig.json* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="27d0f-180">Le code précédent configure le compilateur TypeScript pour produire du code JavaScript compatible [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="27d0f-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="27d0f-181">Créez *src/index.ts* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="27d0f-182">Le code TypeScript précédent récupère les références aux éléments DOM et joint deux gestionnaires d’événements :</span><span class="sxs-lookup"><span data-stu-id="27d0f-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="27d0f-183">`keyup` : Cet événement se déclenche quand l’utilisateur tape des données dans la zone de texte identifiée comme `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="27d0f-184">La fonction `send` est appelée quand l’utilisateur appuie sur la touche **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="27d0f-185">`click` : Cet événement se déclenche quand l’utilisateur clique sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="27d0f-186">La fonction `send` est appelée.</span><span class="sxs-lookup"><span data-stu-id="27d0f-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="27d0f-187">Configurer l’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27d0f-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="27d0f-188">Le code fourni dans la méthode `Startup.Configure` affiche *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="27d0f-189">Remplacez l’appel de méthode `app.Run` par des appels à [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="27d0f-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="27d0f-190">Le code précédent permet au serveur de localiser et traiter le fichier *index.html*, que l’utilisateur entre son URL complète ou l’URL racine de l’application web.</span><span class="sxs-lookup"><span data-stu-id="27d0f-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="27d0f-191">Appelez [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="27d0f-192">Il ajoute les services SignalR à votre projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="27d0f-193">Mappez une route */hub* au hub `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="27d0f-194">Ajoutez les lignes suivantes à la fin de la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="27d0f-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="27d0f-195">Créez un répertoire *Hubs* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="27d0f-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="27d0f-196">Son objectif est de stocker le hub SignalR, qui est créé à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="27d0f-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="27d0f-197">Créez un hub *Hubs/ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="27d0f-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="27d0f-198">Ajoutez le code suivant en haut du fichier *Startup.cs* pour résoudre la référence de `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="27d0f-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="27d0f-199">Activer la communication entre le client et le serveur</span><span class="sxs-lookup"><span data-stu-id="27d0f-199">Enable client and server communication</span></span>

<span data-ttu-id="27d0f-200">Actuellement, l’application affiche un formulaire simple pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="27d0f-201">Rien ne se produit quand vous essayez de le faire.</span><span class="sxs-lookup"><span data-stu-id="27d0f-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="27d0f-202">Le serveur écoute une route spécifique, mais ne fait rien avec les messages envoyés.</span><span class="sxs-lookup"><span data-stu-id="27d0f-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="27d0f-203">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="27d0f-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="27d0f-204">La commande précédente installe le [client SignalR TypeScript](https://www.npmjs.com/package/@aspnet/signalr), ce qui permet au client d’envoyer des messages au serveur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="27d0f-205">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="27d0f-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="27d0f-206">Le code précédent prend en charge la réception de messages du serveur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="27d0f-207">La classe `HubConnectionBuilder` crée un générateur pour configurer la connexion de serveur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="27d0f-208">La fonction `withUrl` configure l’URL du hub.</span><span class="sxs-lookup"><span data-stu-id="27d0f-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="27d0f-209">SignalR permet l’échange de messages entre un client et un serveur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="27d0f-210">Chaque message a un nom spécifique.</span><span class="sxs-lookup"><span data-stu-id="27d0f-210">Each message has a specific name.</span></span> <span data-ttu-id="27d0f-211">Par exemple, vous pouvez avoir des messages avec le nom `messageReceived` qui exécutent la logique chargée d’afficher le nouveau message dans la zone de messages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="27d0f-212">L’écoute d’un message spécifique peut être effectuée au moyen de la fonction `on`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="27d0f-213">Vous pouvez écouter n’importe quel nombre de noms de message.</span><span class="sxs-lookup"><span data-stu-id="27d0f-213">You can listen to any number of message names.</span></span> <span data-ttu-id="27d0f-214">Vous pouvez aussi passer des paramètres au message, comme le nom de l’auteur et le contenu du message reçu.</span><span class="sxs-lookup"><span data-stu-id="27d0f-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="27d0f-215">Dès que le client reçoit un message, un élément `div` est créé avec le nom de l’auteur et le contenu du message dans son attribut `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="27d0f-216">Il est ajouté à l’élément `div` principal qui affiche les messages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="27d0f-217">Maintenant que le client peut recevoir un message, configurez-le pour en envoyer.</span><span class="sxs-lookup"><span data-stu-id="27d0f-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="27d0f-218">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="27d0f-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="27d0f-219">L’envoi d’un message au moyen de la connexion WebSocket nécessite l’appel de la méthode `send`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="27d0f-220">Le premier paramètre de la méthode est le nom du message.</span><span class="sxs-lookup"><span data-stu-id="27d0f-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="27d0f-221">Les données du message se trouvent dans les autres paramètres.</span><span class="sxs-lookup"><span data-stu-id="27d0f-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="27d0f-222">Dans cet exemple, un message identifié comme `newMessage` est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="27d0f-223">Le message se compose du nom d’utilisateur et de l’entrée de l’utilisateur dans une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="27d0f-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="27d0f-224">Si l’envoi fonctionne, la valeur de la zone de texte est effacée.</span><span class="sxs-lookup"><span data-stu-id="27d0f-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="27d0f-225">Ajoutez la méthode en surbrillance à la classe `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="27d0f-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="27d0f-226">Le code précédent diffuse les messages reçus à tous les utilisateurs connectés, une fois que le serveur les reçoit.</span><span class="sxs-lookup"><span data-stu-id="27d0f-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="27d0f-227">Vous n’avez pas besoin d’appeler une méthode générique `on` pour recevoir tous les messages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="27d0f-228">Il vous suffit d’avoir une méthode du même nom que le message.</span><span class="sxs-lookup"><span data-stu-id="27d0f-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="27d0f-229">Dans cet exemple, le client TypeScript envoie un message identifié comme `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="27d0f-230">La méthode C# `NewMessage` attend les données envoyées par le client.</span><span class="sxs-lookup"><span data-stu-id="27d0f-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="27d0f-231">Un appel est effectué à la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) sur [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="27d0f-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="27d0f-232">Les messages reçus sont envoyés à tous les clients connectés au hub.</span><span class="sxs-lookup"><span data-stu-id="27d0f-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="27d0f-233">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="27d0f-233">Test the app</span></span>

<span data-ttu-id="27d0f-234">Vérifiez que l’application fonctionne avec les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="27d0f-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="27d0f-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27d0f-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="27d0f-236">Exécuter Webpack en mode de *mise en production*.</span><span class="sxs-lookup"><span data-stu-id="27d0f-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="27d0f-237">Dans la fenêtre **Console du Gestionnaire de package**, exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="27d0f-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="27d0f-238">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer l’application dans un navigateur sans attacher le débogueur.</span><span class="sxs-lookup"><span data-stu-id="27d0f-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="27d0f-239">Le fichier *wwwroot/index.html* est délivré sous `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="27d0f-240">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="27d0f-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="27d0f-241">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="27d0f-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="27d0f-242">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="27d0f-243">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="27d0f-244">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="27d0f-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="27d0f-245">Exécutez Webpack en mode de *mise en production* en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="27d0f-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="27d0f-246">Générez et exécutez l’application en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="27d0f-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="27d0f-247">Le serveur web démarre l’application et la rend disponible sur localhost.</span><span class="sxs-lookup"><span data-stu-id="27d0f-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="27d0f-248">Ouvrez un navigateur avec l’adresse `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="27d0f-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="27d0f-249">Le fichier *wwwroot/index.html* est présent.</span><span class="sxs-lookup"><span data-stu-id="27d0f-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="27d0f-250">Copiez l’URL à partir de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="27d0f-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="27d0f-251">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="27d0f-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="27d0f-252">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="27d0f-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="27d0f-253">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="27d0f-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="27d0f-254">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="27d0f-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![message affiché dans les deux fenêtres de navigateur](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="27d0f-256">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="27d0f-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
