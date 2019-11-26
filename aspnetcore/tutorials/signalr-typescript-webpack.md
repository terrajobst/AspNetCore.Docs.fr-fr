---
title: Utiliser ASP.NET Core SignalR avec une machine à écrire et un WebPack
author: ssougnez
description: Dans ce didacticiel, vous allez configurer WebPack pour regrouper et créer une ASP.NET Core SignalR application Web dont le client est écrit en écriture manuscrite.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317484"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="65d0d-103">Utiliser ASP.NET Core SignalR avec une machine à écrire et un WebPack</span><span class="sxs-lookup"><span data-stu-id="65d0d-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="65d0d-104">Par [Sébastien Sougnez](https://twitter.com/ssougnez) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="65d0d-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="65d0d-105">[Webpack](https://webpack.js.org/) permet aux développeurs de regrouper et générer les ressources côté client d’une application web.</span><span class="sxs-lookup"><span data-stu-id="65d0d-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="65d0d-106">Ce didacticiel montre comment utiliser WebPack dans une application Web ASP.NET Core SignalR dont le client est écrit dans la [machine à écrire](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="65d0d-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="65d0d-107">Dans ce didacticiel, vous apprendrez à :</span><span class="sxs-lookup"><span data-stu-id="65d0d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="65d0d-108">Génération de modèles automatique d’une application de démarrage ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="65d0d-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="65d0d-109">Configuration de l' SignalR client de machine à écrire</span><span class="sxs-lookup"><span data-stu-id="65d0d-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="65d0d-110">Configurer un pipeline de build à l’aide de Webpack</span><span class="sxs-lookup"><span data-stu-id="65d0d-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="65d0d-111">Configurer le serveur SignalR</span><span class="sxs-lookup"><span data-stu-id="65d0d-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="65d0d-112">Activer la communication entre le client et le serveur</span><span class="sxs-lookup"><span data-stu-id="65d0d-112">Enable communication between client and server</span></span>

<span data-ttu-id="65d0d-113">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="65d0d-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="65d0d-114">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="65d0d-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65d0d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) avec la charge de travail de **développement web et ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="65d0d-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="65d0d-117">SDK .NET Core 3.0 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="65d0d-118">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="65d0d-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="65d0d-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="65d0d-121">SDK .NET Core 3.0 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="65d0d-122">C# pour Visual Studio Code version 1.17.1 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="65d0d-123">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="65d0d-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="65d0d-124">Créer l’application web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65d0d-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65d0d-126">Configurez Visual Studio pour rechercher npm dans la variable d'environnement *PATH*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="65d0d-127">Par défaut, Visual Studio utilise la version de npm qu’il trouve dans son répertoire d’installation.</span><span class="sxs-lookup"><span data-stu-id="65d0d-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="65d0d-128">Suivez ces instructions dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="65d0d-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="65d0d-129">Accédez à **outils** > **options** > **projets et solutions** > **Package Management Web** > **Outils Web externes**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="65d0d-130">Sélectionnez l’entrée *$(PATH)* dans la liste.</span><span class="sxs-lookup"><span data-stu-id="65d0d-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="65d0d-131">Cliquez sur la flèche vers le haut pour déplacer l’entrée à la deuxième position dans la liste.</span><span class="sxs-lookup"><span data-stu-id="65d0d-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuration de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="65d0d-133">La configuration de Visual Studio est terminée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="65d0d-134">Nous allons maintenant créer le projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-134">It's time to create the project.</span></span>

1. <span data-ttu-id="65d0d-135">Utilisez l’option de menu **fichier** > **nouveau** > **projet** et choisissez le modèle **application Web ASP.net Core** .</span><span class="sxs-lookup"><span data-stu-id="65d0d-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="65d0d-136">Nommez le projet *SignalRWebPack*, puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="65d0d-137">Sélectionnez *.net Core* dans la liste déroulante Framework cible, puis sélectionnez *ASP.net Core 3,0* dans la liste déroulante du sélecteur de Framework.</span><span class="sxs-lookup"><span data-stu-id="65d0d-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="65d0d-138">Sélectionnez le modèle **vide** , puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65d0d-140">Exécutez la commande suivante dans le **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="65d0d-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="65d0d-141">Une application web ASP.NET Core vide, ciblant .NET Core, est créée dans un répertoire *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="65d0d-142">Configurer Webpack et TypeScript</span><span class="sxs-lookup"><span data-stu-id="65d0d-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="65d0d-143">Les étapes suivantes configurent la conversion de TypeScript en JavaScript et le regroupement des ressources côté client.</span><span class="sxs-lookup"><span data-stu-id="65d0d-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="65d0d-144">Exécutez la commande suivante à la racine du projet pour créer un fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="65d0d-145">Ajoutez la propriété en surbrillance au fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="65d0d-146">La définition de la propriété `private` sur `true` empêche les avertissements d’installation de package à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="65d0d-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="65d0d-147">Installez les packages npm nécessaires.</span><span class="sxs-lookup"><span data-stu-id="65d0d-147">Install the required npm packages.</span></span> <span data-ttu-id="65d0d-148">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="65d0d-149">Détails de commande à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="65d0d-149">Some command details to note:</span></span>

    * <span data-ttu-id="65d0d-150">Un numéro de version suit le signe `@` pour chaque nom de package.</span><span class="sxs-lookup"><span data-stu-id="65d0d-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="65d0d-151">npm installe ces versions de package spécifiques.</span><span class="sxs-lookup"><span data-stu-id="65d0d-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="65d0d-152">L’option `-E` désactive le comportement par défaut de npm consistant à écrire des opérateurs de plage de [gestion sémantique des versions](https://semver.org/) dans *package.json*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="65d0d-153">Par exemple, `"webpack": "4.29.3"` peut être utilisé à la place de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="65d0d-154">Cette option empêche les mises à niveau involontaires vers des versions de package plus récentes.</span><span class="sxs-lookup"><span data-stu-id="65d0d-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="65d0d-155">Consultez la documentation officielle de [npm-install](https://docs.npmjs.com/cli/install) pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="65d0d-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="65d0d-156">Remplacez la propriété `scripts` du fichier *package.json* par l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="65d0d-157">Explication des scripts :</span><span class="sxs-lookup"><span data-stu-id="65d0d-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="65d0d-158">`build` : Regroupe vos ressources côté client en mode de développement et surveille les changements de fichier.</span><span class="sxs-lookup"><span data-stu-id="65d0d-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="65d0d-159">L’observateur de fichiers force la regénération du regroupement chaque fois qu’un fichier projet change.</span><span class="sxs-lookup"><span data-stu-id="65d0d-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="65d0d-160">L’option `mode` désactive les optimisations de production, comme la minimisation de l’arborescence (tree shaking).</span><span class="sxs-lookup"><span data-stu-id="65d0d-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="65d0d-161">Utilisez uniquement `build` dans le développement.</span><span class="sxs-lookup"><span data-stu-id="65d0d-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="65d0d-162">`release` : Regroupe vos ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="65d0d-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="65d0d-163">`publish` : Exécute le script `release` pour regrouper les ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="65d0d-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="65d0d-164">La commande appelle la commande [publish](/dotnet/core/tools/dotnet-publish) de CLI .NET Core pour publier l’application.</span><span class="sxs-lookup"><span data-stu-id="65d0d-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="65d0d-165">Créez un fichier nommé *webpack.config.js* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="65d0d-166">Le fichier précédent configure la compilation Webpack.</span><span class="sxs-lookup"><span data-stu-id="65d0d-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="65d0d-167">Détails de configuration à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="65d0d-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="65d0d-168">La propriété `output` remplace la valeur par défaut de *dist*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="65d0d-169">Le regroupement est émis dans le répertoire *wwwroot* à la place.</span><span class="sxs-lookup"><span data-stu-id="65d0d-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="65d0d-170">Le tableau `resolve.extensions` comprend *. js* pour importer le code JavaScript du client SignalR.</span><span class="sxs-lookup"><span data-stu-id="65d0d-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="65d0d-171">Créez un répertoire *src* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="65d0d-172">Son objectif est de stocker les ressources côté client du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="65d0d-173">Créez *src/index.html* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="65d0d-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="65d0d-174">Le code HTML précédent définit le balisage réutilisable de la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="65d0d-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="65d0d-175">Créez un répertoire *src/css*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="65d0d-176">Son objectif est de stocker les fichiers *.css* du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="65d0d-177">Créez *src/css/main.css* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="65d0d-178">Le fichier *main.css* précédent définit le style de l’application.</span><span class="sxs-lookup"><span data-stu-id="65d0d-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="65d0d-179">Créez *src/tsconfig.json* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="65d0d-180">Le code précédent configure le compilateur TypeScript pour produire du code JavaScript compatible [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="65d0d-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="65d0d-181">Créez *src/index.ts* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="65d0d-182">Le code TypeScript précédent récupère les références aux éléments DOM et joint deux gestionnaires d’événements :</span><span class="sxs-lookup"><span data-stu-id="65d0d-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="65d0d-183">`keyup` : Cet événement se déclenche quand l’utilisateur tape des données dans la zone de texte identifiée comme `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="65d0d-184">La fonction `send` est appelée quand l’utilisateur appuie sur la touche **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="65d0d-185">`click` : Cet événement se déclenche quand l’utilisateur clique sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="65d0d-186">La fonction `send` est appelée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="65d0d-187">Configurer l’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65d0d-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="65d0d-188">Dans la méthode `Startup.Configure`, ajoutez des appels à [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="65d0d-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="65d0d-189">Le code précédent permet au serveur de localiser et traiter le fichier *index.html*, que l’utilisateur entre son URL complète ou l’URL racine de l’application web.</span><span class="sxs-lookup"><span data-stu-id="65d0d-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="65d0d-190">À la fin de la méthode `Startup.Configure`, mappez un itinéraire */Hub* au concentrateur `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="65d0d-191">Remplacez le code qui affiche *Hello World !*</span><span class="sxs-lookup"><span data-stu-id="65d0d-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="65d0d-192">par la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="65d0d-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="65d0d-193">Dans la méthode `Startup.ConfigureServices`, appelez [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="65d0d-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="65d0d-194">Il ajoute les services SignalR à votre projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="65d0d-195">Créez un répertoire *Hubs* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="65d0d-196">Son objectif est de stocker le Hub SignalR, créé à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="65d0d-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="65d0d-197">Créez un hub *Hubs/ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="65d0d-198">Ajoutez le code suivant en haut du fichier *Startup.cs* pour résoudre la référence de `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="65d0d-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="65d0d-199">Activer la communication entre le client et le serveur</span><span class="sxs-lookup"><span data-stu-id="65d0d-199">Enable client and server communication</span></span>

<span data-ttu-id="65d0d-200">Actuellement, l’application affiche un formulaire simple pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="65d0d-201">Rien ne se produit quand vous essayez de le faire.</span><span class="sxs-lookup"><span data-stu-id="65d0d-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="65d0d-202">Le serveur écoute une route spécifique, mais ne fait rien avec les messages envoyés.</span><span class="sxs-lookup"><span data-stu-id="65d0d-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="65d0d-203">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="65d0d-204">La commande précédente installe la [SignalR client de machine à écrire](https://www.npmjs.com/package/@microsoft/signalr), ce qui permet au client d’envoyer des messages au serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="65d0d-205">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="65d0d-206">Le code précédent prend en charge la réception de messages du serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="65d0d-207">La classe `HubConnectionBuilder` crée un générateur pour configurer la connexion de serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="65d0d-208">La fonction `withUrl` configure l’URL du hub.</span><span class="sxs-lookup"><span data-stu-id="65d0d-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="65d0d-209"> permet l’échange de messages entre un client et un serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="65d0d-210">Chaque message a un nom spécifique.</span><span class="sxs-lookup"><span data-stu-id="65d0d-210">Each message has a specific name.</span></span> <span data-ttu-id="65d0d-211">Par exemple, vous pouvez avoir des messages avec le nom `messageReceived` qui exécutent la logique chargée d’afficher le nouveau message dans la zone de messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="65d0d-212">L’écoute d’un message spécifique peut être effectuée au moyen de la fonction `on`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="65d0d-213">Vous pouvez écouter n’importe quel nombre de noms de message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-213">You can listen to any number of message names.</span></span> <span data-ttu-id="65d0d-214">Vous pouvez aussi passer des paramètres au message, comme le nom de l’auteur et le contenu du message reçu.</span><span class="sxs-lookup"><span data-stu-id="65d0d-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="65d0d-215">Dès que le client reçoit un message, un élément `div` est créé avec le nom de l’auteur et le contenu du message dans son attribut `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="65d0d-216">Il est ajouté à l’élément `div` principal qui affiche les messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="65d0d-217">Maintenant que le client peut recevoir un message, configurez-le pour en envoyer.</span><span class="sxs-lookup"><span data-stu-id="65d0d-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="65d0d-218">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="65d0d-219">L’envoi d’un message au moyen de la connexion WebSocket nécessite l’appel de la méthode `send`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="65d0d-220">Le premier paramètre de la méthode est le nom du message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="65d0d-221">Les données du message se trouvent dans les autres paramètres.</span><span class="sxs-lookup"><span data-stu-id="65d0d-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="65d0d-222">Dans cet exemple, un message identifié comme `newMessage` est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="65d0d-223">Le message se compose du nom d’utilisateur et de l’entrée de l’utilisateur dans une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="65d0d-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="65d0d-224">Si l’envoi fonctionne, la valeur de la zone de texte est effacée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="65d0d-225">Ajoutez la méthode en surbrillance à la classe `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="65d0d-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="65d0d-226">Le code précédent diffuse les messages reçus à tous les utilisateurs connectés, une fois que le serveur les reçoit.</span><span class="sxs-lookup"><span data-stu-id="65d0d-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="65d0d-227">Vous n’avez pas besoin d’appeler une méthode générique `on` pour recevoir tous les messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="65d0d-228">Il vous suffit d’avoir une méthode du même nom que le message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="65d0d-229">Dans cet exemple, le client TypeScript envoie un message identifié comme `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="65d0d-230">La méthode C# `NewMessage` attend les données envoyées par le client.</span><span class="sxs-lookup"><span data-stu-id="65d0d-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="65d0d-231">Un appel est effectué à la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) sur [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="65d0d-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="65d0d-232">Les messages reçus sont envoyés à tous les clients connectés au hub.</span><span class="sxs-lookup"><span data-stu-id="65d0d-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="65d0d-233">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="65d0d-233">Test the app</span></span>

<span data-ttu-id="65d0d-234">Vérifiez que l’application fonctionne avec les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="65d0d-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="65d0d-236">Exécuter Webpack en mode de *mise en production*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="65d0d-237">Dans la fenêtre **Console du Gestionnaire de package**, exécutez la commande suivante à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="65d0d-238">Si vous ne vous trouvez pas à la racine du projet, tapez `cd SignalRWebPack` avant d’entrer la commande.</span><span class="sxs-lookup"><span data-stu-id="65d0d-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="65d0d-239">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer l’application dans un navigateur sans attacher le débogueur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="65d0d-240">Le fichier *wwwroot/index.html* est délivré sous `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="65d0d-241">Si vous recevez des erreurs de compilation, essayez de fermer et de rouvrir la solution.</span><span class="sxs-lookup"><span data-stu-id="65d0d-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="65d0d-242">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="65d0d-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="65d0d-243">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="65d0d-244">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="65d0d-245">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="65d0d-247">Exécutez Webpack en mode de *mise en production* en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="65d0d-248">Générez et exécutez l’application en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="65d0d-249">Le serveur web démarre l’application et la rend disponible sur localhost.</span><span class="sxs-lookup"><span data-stu-id="65d0d-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="65d0d-250">Ouvrez un navigateur avec l’adresse `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="65d0d-251">Le fichier *wwwroot/index.html* est présent.</span><span class="sxs-lookup"><span data-stu-id="65d0d-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="65d0d-252">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="65d0d-253">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="65d0d-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="65d0d-254">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="65d0d-255">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="65d0d-256">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![message affiché dans les deux fenêtres de navigateur](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="65d0d-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) avec la charge de travail de **développement web et ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="65d0d-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="65d0d-260">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="65d0d-261">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="65d0d-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="65d0d-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="65d0d-264">Kit SDK .NET Core 2.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="65d0d-265">C# pour Visual Studio Code version 1.17.1 ou ultérieure</span><span class="sxs-lookup"><span data-stu-id="65d0d-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="65d0d-266">[Node.js](https://nodejs.org/) avec [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="65d0d-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="65d0d-267">Créer l’application web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65d0d-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="65d0d-269">Configurez Visual Studio pour rechercher npm dans la variable d'environnement *PATH*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="65d0d-270">Par défaut, Visual Studio utilise la version de npm qu’il trouve dans son répertoire d’installation.</span><span class="sxs-lookup"><span data-stu-id="65d0d-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="65d0d-271">Suivez ces instructions dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="65d0d-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="65d0d-272">Accédez à **outils** > **options** > **projets et solutions** > **Package Management Web** > **Outils Web externes**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="65d0d-273">Sélectionnez l’entrée *$(PATH)* dans la liste.</span><span class="sxs-lookup"><span data-stu-id="65d0d-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="65d0d-274">Cliquez sur la flèche vers le haut pour déplacer l’entrée à la deuxième position dans la liste.</span><span class="sxs-lookup"><span data-stu-id="65d0d-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configuration de Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="65d0d-276">La configuration de Visual Studio est terminée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="65d0d-277">Nous allons maintenant créer le projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-277">It's time to create the project.</span></span>

1. <span data-ttu-id="65d0d-278">Utilisez l’option de menu **fichier** > **nouveau** > **projet** et choisissez le modèle **application Web ASP.net Core** .</span><span class="sxs-lookup"><span data-stu-id="65d0d-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="65d0d-279">Nommez le projet *SignalRWebPack*, puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="65d0d-280">Sélectionnez *.NET Core* dans la liste déroulante du framework cible, puis *ASP.NET Core 2.2* dans la liste déroulante du sélecteur de framework.</span><span class="sxs-lookup"><span data-stu-id="65d0d-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="65d0d-281">Sélectionnez le modèle **vide** , puis sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="65d0d-283">Exécutez la commande suivante dans le **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="65d0d-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="65d0d-284">Une application web ASP.NET Core vide, ciblant .NET Core, est créée dans un répertoire *SignalRWebPack*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="65d0d-285">Configurer Webpack et TypeScript</span><span class="sxs-lookup"><span data-stu-id="65d0d-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="65d0d-286">Les étapes suivantes configurent la conversion de TypeScript en JavaScript et le regroupement des ressources côté client.</span><span class="sxs-lookup"><span data-stu-id="65d0d-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="65d0d-287">Exécutez la commande suivante à la racine du projet pour créer un fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="65d0d-288">Ajoutez la propriété en surbrillance au fichier *package.json* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="65d0d-289">La définition de la propriété `private` sur `true` empêche les avertissements d’installation de package à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="65d0d-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="65d0d-290">Installez les packages npm nécessaires.</span><span class="sxs-lookup"><span data-stu-id="65d0d-290">Install the required npm packages.</span></span> <span data-ttu-id="65d0d-291">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="65d0d-292">Détails de commande à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="65d0d-292">Some command details to note:</span></span>

    * <span data-ttu-id="65d0d-293">Un numéro de version suit le signe `@` pour chaque nom de package.</span><span class="sxs-lookup"><span data-stu-id="65d0d-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="65d0d-294">npm installe ces versions de package spécifiques.</span><span class="sxs-lookup"><span data-stu-id="65d0d-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="65d0d-295">L’option `-E` désactive le comportement par défaut de npm consistant à écrire des opérateurs de plage de [gestion sémantique des versions](https://semver.org/) dans *package.json*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="65d0d-296">Par exemple, `"webpack": "4.29.3"` peut être utilisé à la place de `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="65d0d-297">Cette option empêche les mises à niveau involontaires vers des versions de package plus récentes.</span><span class="sxs-lookup"><span data-stu-id="65d0d-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="65d0d-298">Consultez la documentation officielle de [npm-install](https://docs.npmjs.com/cli/install) pour plus de détails.</span><span class="sxs-lookup"><span data-stu-id="65d0d-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="65d0d-299">Remplacez la propriété `scripts` du fichier *package.json* par l’extrait de code suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="65d0d-300">Explication des scripts :</span><span class="sxs-lookup"><span data-stu-id="65d0d-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="65d0d-301">`build` : Regroupe vos ressources côté client en mode de développement et surveille les changements de fichier.</span><span class="sxs-lookup"><span data-stu-id="65d0d-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="65d0d-302">L’observateur de fichiers force la regénération du regroupement chaque fois qu’un fichier projet change.</span><span class="sxs-lookup"><span data-stu-id="65d0d-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="65d0d-303">L’option `mode` désactive les optimisations de production, comme la minimisation de l’arborescence (tree shaking).</span><span class="sxs-lookup"><span data-stu-id="65d0d-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="65d0d-304">Utilisez uniquement `build` dans le développement.</span><span class="sxs-lookup"><span data-stu-id="65d0d-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="65d0d-305">`release` : Regroupe vos ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="65d0d-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="65d0d-306">`publish` : Exécute le script `release` pour regrouper les ressources côté client en mode de production.</span><span class="sxs-lookup"><span data-stu-id="65d0d-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="65d0d-307">La commande appelle la commande [publish](/dotnet/core/tools/dotnet-publish) de CLI .NET Core pour publier l’application.</span><span class="sxs-lookup"><span data-stu-id="65d0d-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="65d0d-308">Créez un fichier nommé *webpack.config.js* à la racine du projet avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="65d0d-309">Le fichier précédent configure la compilation Webpack.</span><span class="sxs-lookup"><span data-stu-id="65d0d-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="65d0d-310">Détails de configuration à prendre en compte :</span><span class="sxs-lookup"><span data-stu-id="65d0d-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="65d0d-311">La propriété `output` remplace la valeur par défaut de *dist*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="65d0d-312">Le regroupement est émis dans le répertoire *wwwroot* à la place.</span><span class="sxs-lookup"><span data-stu-id="65d0d-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="65d0d-313">Le tableau `resolve.extensions` comprend *. js* pour importer le code JavaScript du client SignalR.</span><span class="sxs-lookup"><span data-stu-id="65d0d-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="65d0d-314">Créez un répertoire *src* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="65d0d-315">Son objectif est de stocker les ressources côté client du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="65d0d-316">Créez *src/index.html* avec le contenu suivant.</span><span class="sxs-lookup"><span data-stu-id="65d0d-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="65d0d-317">Le code HTML précédent définit le balisage réutilisable de la page d’accueil.</span><span class="sxs-lookup"><span data-stu-id="65d0d-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="65d0d-318">Créez un répertoire *src/css*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="65d0d-319">Son objectif est de stocker les fichiers *.css* du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="65d0d-320">Créez *src/css/main.css* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="65d0d-321">Le fichier *main.css* précédent définit le style de l’application.</span><span class="sxs-lookup"><span data-stu-id="65d0d-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="65d0d-322">Créez *src/tsconfig.json* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="65d0d-323">Le code précédent configure le compilateur TypeScript pour produire du code JavaScript compatible [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="65d0d-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="65d0d-324">Créez *src/index.ts* avec le contenu suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="65d0d-325">Le code TypeScript précédent récupère les références aux éléments DOM et joint deux gestionnaires d’événements :</span><span class="sxs-lookup"><span data-stu-id="65d0d-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="65d0d-326">`keyup` : Cet événement se déclenche quand l’utilisateur tape des données dans la zone de texte identifiée comme `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="65d0d-327">La fonction `send` est appelée quand l’utilisateur appuie sur la touche **Entrée**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="65d0d-328">`click` : Cet événement se déclenche quand l’utilisateur clique sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="65d0d-329">La fonction `send` est appelée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="65d0d-330">Configurer l’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65d0d-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="65d0d-331">Le code fourni dans la méthode `Startup.Configure` affiche *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="65d0d-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="65d0d-332">Remplacez l’appel de méthode `app.Run` par des appels à [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="65d0d-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="65d0d-333">Le code précédent permet au serveur de localiser et traiter le fichier *index.html*, que l’utilisateur entre son URL complète ou l’URL racine de l’application web.</span><span class="sxs-lookup"><span data-stu-id="65d0d-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="65d0d-334">Appelez [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) dans la méthode `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="65d0d-335">Il ajoute les services SignalR à votre projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="65d0d-336">Mappez une route */hub* au hub `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="65d0d-337">Ajoutez les lignes suivantes à la fin de la méthode `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="65d0d-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="65d0d-338">Créez un répertoire *Hubs* à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="65d0d-339">Son objectif est de stocker le Hub SignalR, créé à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="65d0d-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="65d0d-340">Créez un hub *Hubs/ChatHub.cs* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="65d0d-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="65d0d-341">Ajoutez le code suivant en haut du fichier *Startup.cs* pour résoudre la référence de `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="65d0d-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="65d0d-342">Activer la communication entre le client et le serveur</span><span class="sxs-lookup"><span data-stu-id="65d0d-342">Enable client and server communication</span></span>

<span data-ttu-id="65d0d-343">Actuellement, l’application affiche un formulaire simple pour envoyer des messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="65d0d-344">Rien ne se produit quand vous essayez de le faire.</span><span class="sxs-lookup"><span data-stu-id="65d0d-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="65d0d-345">Le serveur écoute une route spécifique, mais ne fait rien avec les messages envoyés.</span><span class="sxs-lookup"><span data-stu-id="65d0d-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="65d0d-346">Exécutez la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="65d0d-347">La commande précédente installe la [SignalR client de machine à écrire](https://www.npmjs.com/package/@microsoft/signalr), ce qui permet au client d’envoyer des messages au serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="65d0d-348">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="65d0d-349">Le code précédent prend en charge la réception de messages du serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="65d0d-350">La classe `HubConnectionBuilder` crée un générateur pour configurer la connexion de serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="65d0d-351">La fonction `withUrl` configure l’URL du hub.</span><span class="sxs-lookup"><span data-stu-id="65d0d-351">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="65d0d-352"> permet l’échange de messages entre un client et un serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-352"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="65d0d-353">Chaque message a un nom spécifique.</span><span class="sxs-lookup"><span data-stu-id="65d0d-353">Each message has a specific name.</span></span> <span data-ttu-id="65d0d-354">Par exemple, vous pouvez avoir des messages avec le nom `messageReceived` qui exécutent la logique chargée d’afficher le nouveau message dans la zone de messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="65d0d-355">L’écoute d’un message spécifique peut être effectuée au moyen de la fonction `on`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="65d0d-356">Vous pouvez écouter n’importe quel nombre de noms de message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-356">You can listen to any number of message names.</span></span> <span data-ttu-id="65d0d-357">Vous pouvez aussi passer des paramètres au message, comme le nom de l’auteur et le contenu du message reçu.</span><span class="sxs-lookup"><span data-stu-id="65d0d-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="65d0d-358">Dès que le client reçoit un message, un élément `div` est créé avec le nom de l’auteur et le contenu du message dans son attribut `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="65d0d-359">Il est ajouté à l’élément `div` principal qui affiche les messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="65d0d-360">Maintenant que le client peut recevoir un message, configurez-le pour en envoyer.</span><span class="sxs-lookup"><span data-stu-id="65d0d-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="65d0d-361">Ajoutez le code en surbrillance au fichier *src/index.ts* :</span><span class="sxs-lookup"><span data-stu-id="65d0d-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="65d0d-362">L’envoi d’un message au moyen de la connexion WebSocket nécessite l’appel de la méthode `send`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="65d0d-363">Le premier paramètre de la méthode est le nom du message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="65d0d-364">Les données du message se trouvent dans les autres paramètres.</span><span class="sxs-lookup"><span data-stu-id="65d0d-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="65d0d-365">Dans cet exemple, un message identifié comme `newMessage` est envoyé au serveur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="65d0d-366">Le message se compose du nom d’utilisateur et de l’entrée de l’utilisateur dans une zone de texte.</span><span class="sxs-lookup"><span data-stu-id="65d0d-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="65d0d-367">Si l’envoi fonctionne, la valeur de la zone de texte est effacée.</span><span class="sxs-lookup"><span data-stu-id="65d0d-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="65d0d-368">Ajoutez la méthode en surbrillance à la classe `ChatHub` :</span><span class="sxs-lookup"><span data-stu-id="65d0d-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="65d0d-369">Le code précédent diffuse les messages reçus à tous les utilisateurs connectés, une fois que le serveur les reçoit.</span><span class="sxs-lookup"><span data-stu-id="65d0d-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="65d0d-370">Vous n’avez pas besoin d’appeler une méthode générique `on` pour recevoir tous les messages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="65d0d-371">Il vous suffit d’avoir une méthode du même nom que le message.</span><span class="sxs-lookup"><span data-stu-id="65d0d-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="65d0d-372">Dans cet exemple, le client TypeScript envoie un message identifié comme `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="65d0d-373">La méthode C# `NewMessage` attend les données envoyées par le client.</span><span class="sxs-lookup"><span data-stu-id="65d0d-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="65d0d-374">Un appel est effectué à la méthode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) sur [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="65d0d-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="65d0d-375">Les messages reçus sont envoyés à tous les clients connectés au hub.</span><span class="sxs-lookup"><span data-stu-id="65d0d-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="65d0d-376">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="65d0d-376">Test the app</span></span>

<span data-ttu-id="65d0d-377">Vérifiez que l’application fonctionne avec les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="65d0d-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="65d0d-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="65d0d-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="65d0d-379">Exécuter Webpack en mode de *mise en production*.</span><span class="sxs-lookup"><span data-stu-id="65d0d-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="65d0d-380">Dans la fenêtre **Console du Gestionnaire de package**, exécutez la commande suivante à la racine du projet.</span><span class="sxs-lookup"><span data-stu-id="65d0d-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="65d0d-381">Si vous ne vous trouvez pas à la racine du projet, tapez `cd SignalRWebPack` avant d’entrer la commande.</span><span class="sxs-lookup"><span data-stu-id="65d0d-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="65d0d-382">Sélectionnez **Déboguer** > **Démarrer sans débogage** pour lancer l’application dans un navigateur sans attacher le débogueur.</span><span class="sxs-lookup"><span data-stu-id="65d0d-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="65d0d-383">Le fichier *wwwroot/index.html* est délivré sous `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="65d0d-384">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="65d0d-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="65d0d-385">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="65d0d-386">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="65d0d-387">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="65d0d-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="65d0d-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="65d0d-389">Exécutez Webpack en mode de *mise en production* en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="65d0d-390">Générez et exécutez l’application en exécutant la commande suivante à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="65d0d-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="65d0d-391">Le serveur web démarre l’application et la rend disponible sur localhost.</span><span class="sxs-lookup"><span data-stu-id="65d0d-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="65d0d-392">Ouvrez un navigateur avec l’adresse `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="65d0d-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="65d0d-393">Le fichier *wwwroot/index.html* est présent.</span><span class="sxs-lookup"><span data-stu-id="65d0d-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="65d0d-394">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="65d0d-395">Ouvrez une autre instance de navigateur (n’importe quel navigateur).</span><span class="sxs-lookup"><span data-stu-id="65d0d-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="65d0d-396">Copiez l’URL de la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="65d0d-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="65d0d-397">Choisissez un navigateur, tapez quelque chose dans la zone de texte **Message**, puis cliquez sur le bouton **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="65d0d-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="65d0d-398">Le nom unique de l’utilisateur et le message sont affichés instantanément dans les deux pages.</span><span class="sxs-lookup"><span data-stu-id="65d0d-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![message affiché dans les deux fenêtres de navigateur](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="65d0d-400">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="65d0d-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
