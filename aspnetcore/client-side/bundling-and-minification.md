---
title: Groupement et la minimisation dans ASP.NET Core
author: scottaddie
description: "Découvrez comment optimiser les ressources statiques dans une application de web ASP.NET Core en appliquant le groupement et la minimisation des techniques."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="61224-103">Groupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-103">Bundling and minification</span></span>

<span data-ttu-id="61224-104">Par [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="61224-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="61224-105">Cet article explique les avantages de l’application de groupement et la minimisation, y compris comment ces fonctionnalités peuvent être utilisées avec les applications web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="61224-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="61224-106">Groupement et la minimisation Nouveautés</span><span class="sxs-lookup"><span data-stu-id="61224-106">What is bundling and minification?</span></span>

<span data-ttu-id="61224-107">Regroupement et la minimisation sont deux optimisations de performances distincts que vous pouvez appliquer dans une application web.</span><span class="sxs-lookup"><span data-stu-id="61224-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="61224-108">Utilisées conjointement, groupement et la minimisation améliorent les performances en réduisant le nombre de demandes de serveur et de réduire la taille des actifs statiques demandés.</span><span class="sxs-lookup"><span data-stu-id="61224-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="61224-109">Groupement et la minimisation principalement améliorent le premier temps de chargement de demande de page.</span><span class="sxs-lookup"><span data-stu-id="61224-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="61224-110">Une fois qu’une page web a été demandée, le navigateur met en cache les ressources statiques (images, CSS et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="61224-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="61224-111">Par conséquent, groupement et la minimisation ne pas améliorer les performances lors de la demande de la même page, ou sur le même site demande les mêmes actifs.</span><span class="sxs-lookup"><span data-stu-id="61224-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="61224-112">Si vous ne définissez pas l’en-tête correctement sur vos éléments multimédias d’expiration, et si vous n’utilisez pas groupement et la minimisation, heuristique d’actualisation du navigateur marque les ressources obsolètes après quelques jours.</span><span class="sxs-lookup"><span data-stu-id="61224-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="61224-113">En outre, le navigateur requiert une demande de validation pour chaque élément multimédia.</span><span class="sxs-lookup"><span data-stu-id="61224-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="61224-114">Dans ce cas, groupement et la minimisation fournissent une amélioration des performances même après la première demande de page.</span><span class="sxs-lookup"><span data-stu-id="61224-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="61224-115">Regroupement</span><span class="sxs-lookup"><span data-stu-id="61224-115">Bundling</span></span>

<span data-ttu-id="61224-116">Regroupement de combine plusieurs fichiers dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="61224-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="61224-117">Regroupement réduit le nombre de demandes de serveur qui sont nécessaires pour afficher une ressource web, par exemple une page web.</span><span class="sxs-lookup"><span data-stu-id="61224-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="61224-118">Vous pouvez créer n’importe quel nombre de lots individuels spécifiquement pour CSS, JavaScript, etc. Moins de fichiers signifie moins de demandes HTTP à partir du navigateur sur le serveur ou dans le service de fourniture de votre application.</span><span class="sxs-lookup"><span data-stu-id="61224-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="61224-119">Il en résulte dans amélioration des performances de charge première page.</span><span class="sxs-lookup"><span data-stu-id="61224-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="61224-120">Minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-120">Minification</span></span>

<span data-ttu-id="61224-121">Minimisation supprime les caractères inutiles à partir de code sans modifier les fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="61224-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="61224-122">Il en résulte une réduction de taille importante de ressources demandées (par exemple, CSS, des images et des fichiers JavaScript).</span><span class="sxs-lookup"><span data-stu-id="61224-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="61224-123">Les effets secondaires communs de minimisation incluent raccourcir les noms de variables pour un caractère et de suppression de commentaires et espaces inutiles.</span><span class="sxs-lookup"><span data-stu-id="61224-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="61224-124">Observez la fonction JavaScript suivante :</span><span class="sxs-lookup"><span data-stu-id="61224-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="61224-125">Minimisation réduit la fonction à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="61224-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="61224-126">Outre la suppression des commentaires et espaces inutiles, les noms de paramètre et variable suivants ont été renommés comme suit :</span><span class="sxs-lookup"><span data-stu-id="61224-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="61224-127">D'origine</span><span class="sxs-lookup"><span data-stu-id="61224-127">Original</span></span> | <span data-ttu-id="61224-128">Affectation d'un nouveau nom</span><span class="sxs-lookup"><span data-stu-id="61224-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="61224-129">Impact de groupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-129">Impact of bundling and minification</span></span>

<span data-ttu-id="61224-130">Le tableau suivant présente les différences entre individuellement le chargement des ressources et à l’aide de groupement et la minimisation :</span><span class="sxs-lookup"><span data-stu-id="61224-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="61224-131">Action</span><span class="sxs-lookup"><span data-stu-id="61224-131">Action</span></span> | <span data-ttu-id="61224-132">B/m</span><span class="sxs-lookup"><span data-stu-id="61224-132">With B/M</span></span> | <span data-ttu-id="61224-133">Sans B/M</span><span class="sxs-lookup"><span data-stu-id="61224-133">Without B/M</span></span> | <span data-ttu-id="61224-134">Modification</span><span class="sxs-lookup"><span data-stu-id="61224-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="61224-135">Demandes de fichiers</span><span class="sxs-lookup"><span data-stu-id="61224-135">File Requests</span></span>  | <span data-ttu-id="61224-136">7</span><span class="sxs-lookup"><span data-stu-id="61224-136">7</span></span>   | <span data-ttu-id="61224-137">18</span><span class="sxs-lookup"><span data-stu-id="61224-137">18</span></span>     | <span data-ttu-id="61224-138">157%</span><span class="sxs-lookup"><span data-stu-id="61224-138">157%</span></span>
<span data-ttu-id="61224-139">Ko transféré</span><span class="sxs-lookup"><span data-stu-id="61224-139">KB Transferred</span></span> | <span data-ttu-id="61224-140">156</span><span class="sxs-lookup"><span data-stu-id="61224-140">156</span></span> | <span data-ttu-id="61224-141">264.68</span><span class="sxs-lookup"><span data-stu-id="61224-141">264.68</span></span> | <span data-ttu-id="61224-142">70%</span><span class="sxs-lookup"><span data-stu-id="61224-142">70%</span></span>
<span data-ttu-id="61224-143">Temps de chargement (ms)</span><span class="sxs-lookup"><span data-stu-id="61224-143">Load Time (ms)</span></span> | <span data-ttu-id="61224-144">885</span><span class="sxs-lookup"><span data-stu-id="61224-144">885</span></span> | <span data-ttu-id="61224-145">2360</span><span class="sxs-lookup"><span data-stu-id="61224-145">2360</span></span>   | <span data-ttu-id="61224-146">167%</span><span class="sxs-lookup"><span data-stu-id="61224-146">167%</span></span>

<span data-ttu-id="61224-147">Les navigateurs sont assez détaillés en ce qui concerne les en-têtes de requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="61224-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="61224-148">Nombre total d’octets envoyés métrique vu une réduction significative lors du regroupement.</span><span class="sxs-lookup"><span data-stu-id="61224-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="61224-149">Le temps de chargement montre une amélioration significative, toutefois, cet exemple s’exécutait localement.</span><span class="sxs-lookup"><span data-stu-id="61224-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="61224-150">Les gains de performances supérieurs sont réalisés lorsque les ressources à l’aide de groupement et la minimisation transférées sur un réseau.</span><span class="sxs-lookup"><span data-stu-id="61224-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="61224-151">Choisir la stratégie de regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="61224-152">Les modèles de projet MVC et les Pages Razor fournissent une solution out of box pour le groupement et la minimisation consistant en un fichier de configuration JSON.</span><span class="sxs-lookup"><span data-stu-id="61224-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="61224-153">Outils tiers, tels que les [Gulp](xref:client-side/using-gulp) et [Grunt](xref:client-side/using-grunt) exécuteurs de tâches, d’accomplir les mêmes tâches avec un peu plus complexe.</span><span class="sxs-lookup"><span data-stu-id="61224-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="61224-154">Un outil tiers est parfait lorsque votre flux de travail de développement requiert le traitement au-delà de groupement et la minimisation&mdash;telles que l’optimisation pelucheux et image.</span><span class="sxs-lookup"><span data-stu-id="61224-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="61224-155">À l’aide de groupement et la minimisation au moment du design, les fichiers réduites sont créés avant le déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="61224-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="61224-156">Groupement et réduction avant le déploiement offre l’avantage d’une charge serveur réduite.</span><span class="sxs-lookup"><span data-stu-id="61224-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="61224-157">Toutefois, il est important de noter ce regroupement au moment du design et minimisation augmente la complexité de la build et fonctionne uniquement avec des fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="61224-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="61224-158">Configurer le regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-158">Configure bundling and minification</span></span>

<span data-ttu-id="61224-159">Les modèles de projet MVC et les Pages Razor fournissent une *bundleconfig.json* fichier de configuration qui définit les options pour chaque regroupement.</span><span class="sxs-lookup"><span data-stu-id="61224-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="61224-160">Par défaut, une configuration de lot unique est définie pour le code JavaScript personnalisé (*wwwroot/js/site.js*) et la feuille de style (*wwwroot/css/site.css*) fichiers :</span><span class="sxs-lookup"><span data-stu-id="61224-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="61224-161">Options de regroupement :</span><span class="sxs-lookup"><span data-stu-id="61224-161">Bundle options include:</span></span>

* <span data-ttu-id="61224-162">`outputFileName`: Le nom du fichier d’offre groupée de sortie.</span><span class="sxs-lookup"><span data-stu-id="61224-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="61224-163">Peut contenir un chemin d’accès relatif à partir de la *bundleconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="61224-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="61224-164">**Obligatoire**</span><span class="sxs-lookup"><span data-stu-id="61224-164">**required**</span></span>
* <span data-ttu-id="61224-165">`inputFiles`: Un tableau de fichiers à regrouper.</span><span class="sxs-lookup"><span data-stu-id="61224-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="61224-166">Voici les chemins d’accès relatifs au fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="61224-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="61224-167">**facultatif**, * une valeur vide entraîne un fichier de sortie vide.</span><span class="sxs-lookup"><span data-stu-id="61224-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="61224-168">[la globalisation](http://www.tldp.org/LDP/abs/html/globbingref.html) modèles sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="61224-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="61224-169">`minify`: Options de réduction pour le type de sortie.</span><span class="sxs-lookup"><span data-stu-id="61224-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="61224-170">**facultatif**, *par défaut :`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="61224-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="61224-171">Options de configuration sont disponibles par type de fichier de sortie.</span><span class="sxs-lookup"><span data-stu-id="61224-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="61224-172">Minimisation CSS</span><span class="sxs-lookup"><span data-stu-id="61224-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="61224-173">Minimisation de JavaScript</span><span class="sxs-lookup"><span data-stu-id="61224-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="61224-174">Minimisation de HTML</span><span class="sxs-lookup"><span data-stu-id="61224-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="61224-175">`includeInProject`: Indicateur indiquant s’il faut ajouter les fichiers générés au fichier projet.</span><span class="sxs-lookup"><span data-stu-id="61224-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="61224-176">**facultatif**, *par défaut : false*</span><span class="sxs-lookup"><span data-stu-id="61224-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="61224-177">`sourceMap`: Indicateur précisant s’il faut générer une carte de code source pour le fichier regroupé.</span><span class="sxs-lookup"><span data-stu-id="61224-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="61224-178">**facultatif**, *par défaut : false*</span><span class="sxs-lookup"><span data-stu-id="61224-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="61224-179">`sourceMapRootPath`: Le chemin d’accès racine pour stocker le fichier de mappage de source.</span><span class="sxs-lookup"><span data-stu-id="61224-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="61224-180">Exécution du moment de la génération du groupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="61224-181">Le [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) package NuGet permet l’exécution de groupement et minimisation au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="61224-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="61224-182">Le package injecte [cibles de MSBuild](/visualstudio/msbuild/msbuild-targets) qui exécuté à la génération et l’heure de nettoyage.</span><span class="sxs-lookup"><span data-stu-id="61224-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="61224-183">Le *bundleconfig.json* fichier est analysé par le processus de génération pour générer les fichiers de sortie en fonction de la configuration définie.</span><span class="sxs-lookup"><span data-stu-id="61224-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="61224-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61224-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="61224-185">Ajouter le *BuildBundlerMinifier* package pour votre projet.</span><span class="sxs-lookup"><span data-stu-id="61224-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="61224-186">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="61224-186">Build the project.</span></span> <span data-ttu-id="61224-187">Le message suivant s’affiche dans la fenêtre Sortie :</span><span class="sxs-lookup"><span data-stu-id="61224-187">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="61224-188">Nettoyer le projet.</span><span class="sxs-lookup"><span data-stu-id="61224-188">Clean the project.</span></span> <span data-ttu-id="61224-189">Le message suivant s’affiche dans la fenêtre Sortie :</span><span class="sxs-lookup"><span data-stu-id="61224-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="61224-190">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="61224-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="61224-191">Ajouter le *BuildBundlerMinifier* package pour votre projet :</span><span class="sxs-lookup"><span data-stu-id="61224-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="61224-192">Si vous utilisez ASP.NET Core 1.x, restaurer le package nouvellement ajouté :</span><span class="sxs-lookup"><span data-stu-id="61224-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="61224-193">Générez le projet :</span><span class="sxs-lookup"><span data-stu-id="61224-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="61224-194">Les options suivantes apparaissent :</span><span class="sxs-lookup"><span data-stu-id="61224-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="61224-195">Nettoyer le projet :</span><span class="sxs-lookup"><span data-stu-id="61224-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="61224-196">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="61224-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="61224-197">Exécution ad hoc de groupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="61224-198">Il est possible d’exécuter les tâches de groupement et la minimisation sur une base ad hoc, sans générer le projet.</span><span class="sxs-lookup"><span data-stu-id="61224-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="61224-199">Ajouter le [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) package NuGet à votre projet :</span><span class="sxs-lookup"><span data-stu-id="61224-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="61224-200">Ce package étend .NET Core CLI pour inclure la *offre groupée-dotnet* outil.</span><span class="sxs-lookup"><span data-stu-id="61224-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="61224-201">La commande suivante peut être exécutée dans la fenêtre de Console de gestionnaire de Package (PMC) ou dans une invite de commandes :</span><span class="sxs-lookup"><span data-stu-id="61224-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="61224-202">Gestionnaire de Package NuGet ajoute les dépendances vers le fichier *.csproj comme `<PackageReference />` nœuds.</span><span class="sxs-lookup"><span data-stu-id="61224-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="61224-203">Le `dotnet bundle` commande est inscrit avec l’interface de ligne de base .NET uniquement lorsqu’un `<DotNetCliToolReference />` nœud est utilisé.</span><span class="sxs-lookup"><span data-stu-id="61224-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="61224-204">Modifiez le fichier *.csproj en conséquence.</span><span class="sxs-lookup"><span data-stu-id="61224-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="61224-205">Ajouter des fichiers à un flux de travail</span><span class="sxs-lookup"><span data-stu-id="61224-205">Add files to workflow</span></span>

<span data-ttu-id="61224-206">Prenons un exemple dans lequel un autre *custom.css* fichier est ajouté qui ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="61224-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="61224-207">Pour minimiser *custom.css* et regrouper avec *site.css* dans un *site.min.css* , ajoutez le chemin d’accès relatif à *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="61224-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="61224-208">Vous pouvez également le modèle de la globalisation suivants peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="61224-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="61224-209">Ce modèle de la globalisation correspond à tous les fichiers CSS et exclut le modèle de fichier réduite.</span><span class="sxs-lookup"><span data-stu-id="61224-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="61224-210">Générez l'application.</span><span class="sxs-lookup"><span data-stu-id="61224-210">Build the application.</span></span> <span data-ttu-id="61224-211">Ouvrez *site.min.css* et notez le contenu de *custom.css* est ajouté à la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="61224-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="61224-212">Groupement et minimisation basée sur l’environnement</span><span class="sxs-lookup"><span data-stu-id="61224-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="61224-213">Comme meilleure pratique, les fichiers regroupés et réduites de votre application doivent être utilisés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="61224-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="61224-214">Pendant le développement, les fichiers d’origine apporter pour faciliter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="61224-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="61224-215">Spécifier les fichiers à inclure dans vos pages à l’aide de la [assistance de balise d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) dans les vues.</span><span class="sxs-lookup"><span data-stu-id="61224-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="61224-216">L’application d’assistance de balise environnement restitue uniquement son contenu lors de l’exécution dans spécifiques [environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="61224-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="61224-217">Les éléments suivants `environment` effectue le rendu lors de l’exécution les fichiers CSS non traités le `Development` environnement :</span><span class="sxs-lookup"><span data-stu-id="61224-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61224-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61224-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61224-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61224-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="61224-220">Les éléments suivants `environment` effectue le rendu lors de l’exécution dans un environnement autre que les fichiers CSS groupées et réduites `Development`.</span><span class="sxs-lookup"><span data-stu-id="61224-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="61224-221">Par exemple, en cours d’exécution `Production` ou `Staging` déclenche le rendu de ces feuilles de style :</span><span class="sxs-lookup"><span data-stu-id="61224-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="61224-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="61224-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="61224-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="61224-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="61224-224">Consommer bundleconfig.json de Gulp</span><span class="sxs-lookup"><span data-stu-id="61224-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="61224-225">Il existe des cas dans lequel les flux de travail groupement et la minimisation d’une application nécessite un traitement supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="61224-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="61224-226">Optimisation des images, fonctions antispam de cache et le traitement d’asset CDN sont des exemples.</span><span class="sxs-lookup"><span data-stu-id="61224-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="61224-227">Pour satisfaire ces exigences, vous pouvez convertir le flux de travail groupement et la minimisation pour utiliser Gulp.</span><span class="sxs-lookup"><span data-stu-id="61224-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="61224-228">Utiliser l’extension textile et minimisation</span><span class="sxs-lookup"><span data-stu-id="61224-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="61224-229">Visual Studio [textile & minimisation](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension gère la conversion Gulp.</span><span class="sxs-lookup"><span data-stu-id="61224-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="61224-230">Avec le bouton droit le *bundleconfig.json* fichier dans l’Explorateur de solutions et sélectionnez **textile & minimisation** > **convertir à Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="61224-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Convertir à Gulp un élément de menu contextuel](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="61224-232">Le *gulpfile.js* et *package.json* fichiers sont ajoutés au projet.</span><span class="sxs-lookup"><span data-stu-id="61224-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="61224-233">La prise en charge [npm](https://www.npmjs.com/) packages répertoriés dans le *package.json* du fichier `devDependencies` section sont installés.</span><span class="sxs-lookup"><span data-stu-id="61224-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="61224-234">Dans la fenêtre PMC pour installer l’interface CLI Gulp comme dépendance globale, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="61224-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="61224-235">Le *gulpfile.js* lecture des fichiers du *bundleconfig.json* fichier pour les entrées, sorties et les paramètres.</span><span class="sxs-lookup"><span data-stu-id="61224-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="61224-236">Convertir manuellement</span><span class="sxs-lookup"><span data-stu-id="61224-236">Convert manually</span></span>

<span data-ttu-id="61224-237">Si l’extension du programme d’installation regroupé & minimisation et/ou de Visual Studio ne sont pas disponible, convertir manuellement.</span><span class="sxs-lookup"><span data-stu-id="61224-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="61224-238">Ajouter un *package.json* fichier, par le code suivant `devDependencies`, à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="61224-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="61224-239">Installer les dépendances en exécutant la commande suivante au même niveau que *package.json*:</span><span class="sxs-lookup"><span data-stu-id="61224-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="61224-240">Installez l’interface CLI Gulp comme une dépendance globale :</span><span class="sxs-lookup"><span data-stu-id="61224-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="61224-241">Copie le *gulpfile.js* le fichier sous la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="61224-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="61224-242">Exécuter des tâches de Gulp</span><span class="sxs-lookup"><span data-stu-id="61224-242">Run Gulp tasks</span></span>

<span data-ttu-id="61224-243">Pour déclencher la tâche de réduction Gulp avant le projet dans Visual Studio, ajoutez le code suivant [cible MSBuild](/visualstudio/msbuild/msbuild-targets) au fichier *.csproj :</span><span class="sxs-lookup"><span data-stu-id="61224-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="61224-244">Dans cet exemple, toutes les tâches définies dans le `MyPreCompileTarget` cible s’exécutent avant prédéfinis `Build` cible.</span><span class="sxs-lookup"><span data-stu-id="61224-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="61224-245">Sortie semblable au suivant s’affiche dans la fenêtre de sortie de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="61224-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="61224-246">Également, Explorateur d’exécuteur de tâche de Visual Studio peuvent être utilisée pour lier des tâches de Gulp à des événements spécifiques de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="61224-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="61224-247">Consultez [en cours d’exécution des tâches par défaut](xref:client-side/using-gulp#running-default-tasks) pour obtenir des instructions sur cela.</span><span class="sxs-lookup"><span data-stu-id="61224-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61224-248">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="61224-248">Additional resources</span></span>

* [<span data-ttu-id="61224-249">Utilisation de Gulp</span><span class="sxs-lookup"><span data-stu-id="61224-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="61224-250">Utilisation de Grunt</span><span class="sxs-lookup"><span data-stu-id="61224-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="61224-251">Utilisation de plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="61224-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="61224-252">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="61224-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
