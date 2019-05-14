---
title: Regrouper et minimiser les ressources statiques dans ASP.NET Core
author: scottaddie
description: Découvrez comment optimiser les ressources statiques dans une application web ASP.NET Core en appliquant les techniques de regroupement et minimisation.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: ba01d365a25dfbd13fed89263d7489b2ce2a8771
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535930"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="146e2-103">Regrouper et minimiser les ressources statiques dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="146e2-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="146e2-104">Par [Scott Addie](https://twitter.com/Scott_Addie) et [David PIN](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="146e2-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="146e2-105">Cet article explique les avantages de l’application de regroupement et minimisation, y compris comment ces fonctionnalités peuvent être utilisées avec les applications web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="146e2-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="146e2-106">Qu’est le regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-106">What is bundling and minification</span></span>

<span data-ttu-id="146e2-107">Regroupement et minimisation sont deux optimisations de performances distincts que vous pouvez appliquer dans une application web.</span><span class="sxs-lookup"><span data-stu-id="146e2-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="146e2-108">Utilisés ensemble, regroupement et minimisation améliorent les performances en réduisant le nombre de demandes de serveur et de réduire la taille des actifs statiques demandés.</span><span class="sxs-lookup"><span data-stu-id="146e2-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="146e2-109">Regroupement et minimisation principalement améliorent la première fois de charge de demande de page.</span><span class="sxs-lookup"><span data-stu-id="146e2-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="146e2-110">Une fois qu’une page web a été demandée, le navigateur met en cache les ressources statiques (JavaScript, CSS et images).</span><span class="sxs-lookup"><span data-stu-id="146e2-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="146e2-111">Par conséquent, regroupement et minimisation ne pas améliorer les performances lors de la demande de même, les pages, sur le même site demande les mêmes ressources.</span><span class="sxs-lookup"><span data-stu-id="146e2-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="146e2-112">Si l’expiration en-tête n’est pas défini correctement sur les ressources et si le regroupement et minimisation n’est pas utilisé, heuristique d’actualisation du navigateur marque les ressources obsolètes après quelques jours.</span><span class="sxs-lookup"><span data-stu-id="146e2-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="146e2-113">En outre, le navigateur requiert une demande de validation pour chaque élément multimédia.</span><span class="sxs-lookup"><span data-stu-id="146e2-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="146e2-114">Dans ce cas, regroupement et minimisation fournissent une amélioration des performances même après la première demande de page.</span><span class="sxs-lookup"><span data-stu-id="146e2-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="146e2-115">Le regroupement</span><span class="sxs-lookup"><span data-stu-id="146e2-115">Bundling</span></span>

<span data-ttu-id="146e2-116">Regroupement de combiner plusieurs fichiers dans un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="146e2-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="146e2-117">Regroupement réduit le nombre de demandes de serveur qui sont nécessaires à l’affichage d’une ressource web, telle qu’une page web.</span><span class="sxs-lookup"><span data-stu-id="146e2-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="146e2-118">Vous pouvez créer n’importe quel nombre de regroupements individuels spécifiquement pour CSS, JavaScript, etc. Moins de fichiers signifie moins de demandes HTTP à partir du navigateur au serveur ou à partir du service fournissant à votre application.</span><span class="sxs-lookup"><span data-stu-id="146e2-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="146e2-119">Il en résulte amélioration des performances de charge première page.</span><span class="sxs-lookup"><span data-stu-id="146e2-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="146e2-120">Minification</span><span class="sxs-lookup"><span data-stu-id="146e2-120">Minification</span></span>

<span data-ttu-id="146e2-121">Minimisation supprime les caractères inutiles à partir du code sans dégrader les fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="146e2-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="146e2-122">Il en résulte une réduction de taille importante dans les ressources demandées (par exemple, images, fichiers CSS et JavaScript).</span><span class="sxs-lookup"><span data-stu-id="146e2-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="146e2-123">Les effets secondaires communs de minimisation incluent raccourcir les noms de variables pour un caractère et de suppression de commentaires et espace blanc inutile.</span><span class="sxs-lookup"><span data-stu-id="146e2-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="146e2-124">Considérez la fonction JavaScript suivante :</span><span class="sxs-lookup"><span data-stu-id="146e2-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="146e2-125">Minimisation réduit la fonction à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="146e2-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="146e2-126">Outre la suppression de commentaires et l’espace blanc inutile, les noms de paramètre et sa variable suivants ont été renommés comme suit :</span><span class="sxs-lookup"><span data-stu-id="146e2-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="146e2-127">D'origine</span><span class="sxs-lookup"><span data-stu-id="146e2-127">Original</span></span> | <span data-ttu-id="146e2-128">Affectation d'un nouveau nom</span><span class="sxs-lookup"><span data-stu-id="146e2-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="146e2-129">Impact de regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-129">Impact of bundling and minification</span></span>

<span data-ttu-id="146e2-130">Le tableau suivant présente les différences entre le chargement de ressources et à l’aide de regroupement et minimisation individuellement :</span><span class="sxs-lookup"><span data-stu-id="146e2-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="146e2-131">Action</span><span class="sxs-lookup"><span data-stu-id="146e2-131">Action</span></span> | <span data-ttu-id="146e2-132">Avec B/M</span><span class="sxs-lookup"><span data-stu-id="146e2-132">With B/M</span></span> | <span data-ttu-id="146e2-133">Sans B/M</span><span class="sxs-lookup"><span data-stu-id="146e2-133">Without B/M</span></span> | <span data-ttu-id="146e2-134">Modification</span><span class="sxs-lookup"><span data-stu-id="146e2-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="146e2-135">Demandes de fichiers</span><span class="sxs-lookup"><span data-stu-id="146e2-135">File Requests</span></span>  | <span data-ttu-id="146e2-136">7</span><span class="sxs-lookup"><span data-stu-id="146e2-136">7</span></span>   | <span data-ttu-id="146e2-137">18</span><span class="sxs-lookup"><span data-stu-id="146e2-137">18</span></span>     | <span data-ttu-id="146e2-138">157%</span><span class="sxs-lookup"><span data-stu-id="146e2-138">157%</span></span>
<span data-ttu-id="146e2-139">Ko transférée</span><span class="sxs-lookup"><span data-stu-id="146e2-139">KB Transferred</span></span> | <span data-ttu-id="146e2-140">156</span><span class="sxs-lookup"><span data-stu-id="146e2-140">156</span></span> | <span data-ttu-id="146e2-141">264.68</span><span class="sxs-lookup"><span data-stu-id="146e2-141">264.68</span></span> | <span data-ttu-id="146e2-142">70%</span><span class="sxs-lookup"><span data-stu-id="146e2-142">70%</span></span>
<span data-ttu-id="146e2-143">Temps de chargement (ms)</span><span class="sxs-lookup"><span data-stu-id="146e2-143">Load Time (ms)</span></span> | <span data-ttu-id="146e2-144">885</span><span class="sxs-lookup"><span data-stu-id="146e2-144">885</span></span> | <span data-ttu-id="146e2-145">2360</span><span class="sxs-lookup"><span data-stu-id="146e2-145">2360</span></span>   | <span data-ttu-id="146e2-146">167%</span><span class="sxs-lookup"><span data-stu-id="146e2-146">167%</span></span>

<span data-ttu-id="146e2-147">Les navigateurs sont assez détaillés en ce qui concerne les en-têtes de requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="146e2-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="146e2-148">Le nombre total d’octets envoyés métrique vu une réduction significative lors du regroupement.</span><span class="sxs-lookup"><span data-stu-id="146e2-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="146e2-149">Le temps de chargement montre une amélioration significative, mais cet exemple a été exécuté localement.</span><span class="sxs-lookup"><span data-stu-id="146e2-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="146e2-150">Gains de performances sont réalisés lorsque des composants à l’aide de regroupement et minimisation transférées sur un réseau.</span><span class="sxs-lookup"><span data-stu-id="146e2-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="146e2-151">Choisir une stratégie de regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="146e2-152">Les modèles de projet MVC et les Pages Razor fournissent une solution out-of-the-box pour le regroupement et minimisation consistant en un fichier de configuration JSON.</span><span class="sxs-lookup"><span data-stu-id="146e2-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="146e2-153">Outils tiers, tels que le [Grunt](xref:client-side/using-grunt) exécuteur de tâches, d’accomplir les mêmes tâches avec un peu plus complexe.</span><span class="sxs-lookup"><span data-stu-id="146e2-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="146e2-154">Un outil tiers est idéaux lorsque votre flux de travail de développement requiert un traitement au-delà de regroupement et minimisation&mdash;telles que l’optimisation lint et image.</span><span class="sxs-lookup"><span data-stu-id="146e2-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="146e2-155">À l’aide de regroupement et minimisation au moment du design, les fichiers réduits sont créés avant le déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="146e2-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="146e2-156">Bundles et minimisation avant le déploiement offre l’avantage d’une charge serveur réduite.</span><span class="sxs-lookup"><span data-stu-id="146e2-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="146e2-157">Toutefois, il est important de reconnaître ce regroupement au moment du design et de minimisation augmente la complexité de la build et fonctionne uniquement avec les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="146e2-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="146e2-158">Configurer le regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="146e2-159">Dans ASP.NET Core 2.0 ou version antérieure, les modèles de projet MVC et les Pages Razor fournissent une *bundleconfig.json* fichier de configuration qui définit les options pour chaque regroupement :</span><span class="sxs-lookup"><span data-stu-id="146e2-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="146e2-160">Dans ASP.NET Core 2.1 ou version ultérieure, ajoutez un nouveau fichier JSON, nommé *bundleconfig.json*, à la racine du projet MVC ou Pages Razor.</span><span class="sxs-lookup"><span data-stu-id="146e2-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="146e2-161">Inclure le code JSON suivant dans ce fichier comme point de départ :</span><span class="sxs-lookup"><span data-stu-id="146e2-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="146e2-162">Le *bundleconfig.json* fichier définit les options pour chaque regroupement.</span><span class="sxs-lookup"><span data-stu-id="146e2-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="146e2-163">Dans l’exemple précédent, une configuration de lot unique est définie pour le code JavaScript personnalisé (*wwwroot/js/site.js*) et la feuille de style (*wwwroot/css/site.css*) les fichiers.</span><span class="sxs-lookup"><span data-stu-id="146e2-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="146e2-164">Options de configuration sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="146e2-164">Configuration options include:</span></span>

* <span data-ttu-id="146e2-165">`outputFileName`: Le nom de fichier d’offre groupée de sortie.</span><span class="sxs-lookup"><span data-stu-id="146e2-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="146e2-166">Peut contenir un chemin d’accès relatif à partir de la *bundleconfig.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="146e2-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="146e2-167">**required**</span><span class="sxs-lookup"><span data-stu-id="146e2-167">**required**</span></span>
* <span data-ttu-id="146e2-168">`inputFiles`: Un tableau de fichiers à regrouper.</span><span class="sxs-lookup"><span data-stu-id="146e2-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="146e2-169">Il s’agit des chemins d’accès relatifs au fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="146e2-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="146e2-170">**facultatif**, \* une valeur vide résulte en un fichier de sortie vide.</span><span class="sxs-lookup"><span data-stu-id="146e2-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="146e2-171">[utilisation des caractères génériques](http://www.tldp.org/LDP/abs/html/globbingref.html) modèles sont pris en charge.</span><span class="sxs-lookup"><span data-stu-id="146e2-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="146e2-172">`minify`: Les options de minimisation pour le type de sortie.</span><span class="sxs-lookup"><span data-stu-id="146e2-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="146e2-173">**optional**, *default - `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="146e2-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="146e2-174">Options de configuration sont disponibles par type de fichier de sortie.</span><span class="sxs-lookup"><span data-stu-id="146e2-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="146e2-175">Minimisation CSS</span><span class="sxs-lookup"><span data-stu-id="146e2-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="146e2-176">Minimisation de JavaScript</span><span class="sxs-lookup"><span data-stu-id="146e2-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="146e2-177">Minimisation de HTML</span><span class="sxs-lookup"><span data-stu-id="146e2-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="146e2-178">`includeInProject`: Indicateur précisant s’il faut ajouter les fichiers générés au fichier projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="146e2-179">**optional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="146e2-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="146e2-180">`sourceMap`: Indicateur précisant s’il faut générer une carte de code source pour le fichier regroupé.</span><span class="sxs-lookup"><span data-stu-id="146e2-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="146e2-181">**optional**, *default - false*</span><span class="sxs-lookup"><span data-stu-id="146e2-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="146e2-182">`sourceMapRootPath`: Le chemin d’accès racine pour stocker le fichier de mappage de source généré.</span><span class="sxs-lookup"><span data-stu-id="146e2-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="146e2-183">Exécution du moment de la génération de regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="146e2-184">Le [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) package NuGet permet l’exécution de regroupement et minimisation au moment de la génération.</span><span class="sxs-lookup"><span data-stu-id="146e2-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="146e2-185">Le package injecte [cibles MSBuild](/visualstudio/msbuild/msbuild-targets) qui exécuté à la génération et l’heure propre.</span><span class="sxs-lookup"><span data-stu-id="146e2-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="146e2-186">Le *bundleconfig.json* fichier est analysé par le processus de génération pour générer les fichiers de sortie selon la configuration définie.</span><span class="sxs-lookup"><span data-stu-id="146e2-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="146e2-187">BuildBundlerMinifier appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support.</span><span class="sxs-lookup"><span data-stu-id="146e2-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="146e2-188">Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="146e2-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="146e2-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="146e2-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="146e2-190">Ajouter le *BuildBundlerMinifier* package à votre projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="146e2-191">Générez le projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-191">Build the project.</span></span> <span data-ttu-id="146e2-192">Le message suivant s’affiche dans la fenêtre Sortie :</span><span class="sxs-lookup"><span data-stu-id="146e2-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="146e2-193">Nettoyer le projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-193">Clean the project.</span></span> <span data-ttu-id="146e2-194">Le message suivant s’affiche dans la fenêtre Sortie :</span><span class="sxs-lookup"><span data-stu-id="146e2-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="146e2-195">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="146e2-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="146e2-196">Ajouter le *BuildBundlerMinifier* package à votre projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="146e2-197">Si vous utilisez ASP.NET Core 1.x, restaurer le package nouvellement ajouté :</span><span class="sxs-lookup"><span data-stu-id="146e2-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="146e2-198">Générez le projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-198">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="146e2-199">Ce qui suit s’affiche :</span><span class="sxs-lookup"><span data-stu-id="146e2-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="146e2-200">Nettoyer le projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-200">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="146e2-201">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="146e2-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="146e2-202">Exécution ad hoc de regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="146e2-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="146e2-203">Il est possible d’exécuter les tâches de regroupement et minimisation sur une base ad hoc, sans générer le projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="146e2-204">Ajouter le [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) package NuGet à votre projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="146e2-205">BundlerMinifier.Core appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support.</span><span class="sxs-lookup"><span data-stu-id="146e2-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="146e2-206">Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="146e2-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="146e2-207">Ce package étend l’interface CLI .NET Core pour inclure le *dotnet-bundle* outil.</span><span class="sxs-lookup"><span data-stu-id="146e2-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="146e2-208">La commande suivante peut être exécutée dans la fenêtre de Console de gestionnaire de Package (PMC) ou dans un interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="146e2-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="146e2-209">Gestionnaire de Package NuGet ajoute des dépendances au fichier \*.csproj comme `<PackageReference />` nœuds.</span><span class="sxs-lookup"><span data-stu-id="146e2-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="146e2-210">Le `dotnet bundle` commande est inscrit avec l’interface CLI .NET Core uniquement quand un `<DotNetCliToolReference />` nœud est utilisé.</span><span class="sxs-lookup"><span data-stu-id="146e2-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="146e2-211">Modifiez le fichier \*.csproj en conséquence.</span><span class="sxs-lookup"><span data-stu-id="146e2-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="146e2-212">Ajouter des fichiers à un flux de travail</span><span class="sxs-lookup"><span data-stu-id="146e2-212">Add files to workflow</span></span>

<span data-ttu-id="146e2-213">Prenons un exemple dans lequel un autre *custom.css* fichier est ajouté, qui ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="146e2-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="146e2-214">À minimiser *custom.css* et la regrouper avec *site.css* dans un *site.min.css* , ajoutez le chemin d’accès relatif à *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="146e2-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="146e2-215">Vous pouvez également le modèle d’utilisation des caractères génériques suivants peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="146e2-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="146e2-216">Ce modèle de globbing correspond à tous les fichiers CSS et exclut le modèle de fichier réduit.</span><span class="sxs-lookup"><span data-stu-id="146e2-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="146e2-217">Générez l'application.</span><span class="sxs-lookup"><span data-stu-id="146e2-217">Build the application.</span></span> <span data-ttu-id="146e2-218">Ouvrez *site.min.css* et notez le contenu de *custom.css* est ajouté à la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="146e2-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="146e2-219">Regroupement et minimisation basé sur l’environnement</span><span class="sxs-lookup"><span data-stu-id="146e2-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="146e2-220">Comme meilleure pratique, les fichiers regroupés et minimisés de votre application doivent être utilisés dans un environnement de production.</span><span class="sxs-lookup"><span data-stu-id="146e2-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="146e2-221">Pendant le développement, les fichiers d’origine apporter pour faciliter le débogage de l’application.</span><span class="sxs-lookup"><span data-stu-id="146e2-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="146e2-222">Spécifier les fichiers à inclure dans vos pages à l’aide de la [Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) dans vos vues.</span><span class="sxs-lookup"><span data-stu-id="146e2-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="146e2-223">Le Tag Helper environnement restitue uniquement son contenu lors de l’exécution en particulier [environnements](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="146e2-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="146e2-224">Ce qui suit `environment` balise restitue les fichiers CSS non traités lors de l’exécution le `Development` environnement :</span><span class="sxs-lookup"><span data-stu-id="146e2-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="146e2-225">Ce qui suit `environment` effectue le rendu lors de l’exécution dans un environnement autre que les fichiers CSS regroupés et minimisés `Development`.</span><span class="sxs-lookup"><span data-stu-id="146e2-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="146e2-226">Par exemple, en cours d’exécution `Production` ou `Staging` déclenche le rendu de ces feuilles de style :</span><span class="sxs-lookup"><span data-stu-id="146e2-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="146e2-227">Consommer bundleconfig.json à partir de Gulp</span><span class="sxs-lookup"><span data-stu-id="146e2-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="146e2-228">Il existe des cas dans lequel les flux de travail regroupement et minimisation d’une application nécessite un traitement supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="146e2-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="146e2-229">Exemples : optimisation d’image, cache busting et traitement actif CDN.</span><span class="sxs-lookup"><span data-stu-id="146e2-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="146e2-230">Pour satisfaire ces exigences, vous pouvez convertir le flux de travail de regroupement et minimisation pour utiliser Gulp.</span><span class="sxs-lookup"><span data-stu-id="146e2-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="146e2-231">Utiliser l’extension Bundler & Minifier</span><span class="sxs-lookup"><span data-stu-id="146e2-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="146e2-232">Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension gère la conversion en Gulp.</span><span class="sxs-lookup"><span data-stu-id="146e2-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="146e2-233">L’extension Bundler & Minifier appartient à un projet communautaire sur GitHub pour laquelle Microsoft ne fournit aucun support.</span><span class="sxs-lookup"><span data-stu-id="146e2-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="146e2-234">Soumettre des problèmes [ici](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="146e2-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="146e2-235">Avec le bouton droit le *bundleconfig.json* fichier dans l’Explorateur de solutions et sélectionnez **Bundler & Minifier** > **convertir à Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="146e2-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Élément de menu contextuel de convertir le Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="146e2-237">Le *gulpfile.js* et *package.json* fichiers sont ajoutés au projet.</span><span class="sxs-lookup"><span data-stu-id="146e2-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="146e2-238">La prise en charge [npm](https://www.npmjs.com/) packages répertoriés dans le *package.json* du fichier `devDependencies` section sont installés.</span><span class="sxs-lookup"><span data-stu-id="146e2-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="146e2-239">Exécutez la commande suivante dans la fenêtre PMC pour installer l’interface CLI de Gulp comme dépendance globale :</span><span class="sxs-lookup"><span data-stu-id="146e2-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="146e2-240">Le *gulpfile.js* lectures de fichiers le *bundleconfig.json* fichier pour les entrées, sorties et des paramètres.</span><span class="sxs-lookup"><span data-stu-id="146e2-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="146e2-241">Convertir manuellement</span><span class="sxs-lookup"><span data-stu-id="146e2-241">Convert manually</span></span>

<span data-ttu-id="146e2-242">Si Visual Studio et/ou l’extension Bundler & Minifier ne sont pas disponible, convertir manuellement.</span><span class="sxs-lookup"><span data-stu-id="146e2-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="146e2-243">Ajouter un *package.json* fichier, par le code suivant `devDependencies`, à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="146e2-244">Installer les dépendances en exécutant la commande suivante au même niveau que *package.json*:</span><span class="sxs-lookup"><span data-stu-id="146e2-244">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="146e2-245">Installez l’interface CLI de Gulp comme une dépendance globale :</span><span class="sxs-lookup"><span data-stu-id="146e2-245">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="146e2-246">Copie le *gulpfile.js* fichier ci-dessous à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="146e2-246">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="146e2-247">Exécuter des tâches Gulp</span><span class="sxs-lookup"><span data-stu-id="146e2-247">Run Gulp tasks</span></span>

<span data-ttu-id="146e2-248">Pour déclencher la tâche de minimisation Gulp avant la génération du projet dans Visual Studio, ajoutez le code suivant [cible MSBuild](/visualstudio/msbuild/msbuild-targets) au fichier \*.csproj :</span><span class="sxs-lookup"><span data-stu-id="146e2-248">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="146e2-249">Dans cet exemple, toutes les tâches définies dans le `MyPreCompileTarget` cible à exécuter avant le texte prédéfini `Build` cible.</span><span class="sxs-lookup"><span data-stu-id="146e2-249">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="146e2-250">Sortie similaire à ce qui suit s’affiche dans la fenêtre de sortie de Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="146e2-250">Output similar to the following appears in Visual Studio's Output window:</span></span>

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


## <a name="additional-resources"></a><span data-ttu-id="146e2-251">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="146e2-251">Additional resources</span></span>

* [<span data-ttu-id="146e2-252">Utiliser Grunt</span><span class="sxs-lookup"><span data-stu-id="146e2-252">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="146e2-253">Utiliser plusieurs environnements</span><span class="sxs-lookup"><span data-stu-id="146e2-253">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="146e2-254">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="146e2-254">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
