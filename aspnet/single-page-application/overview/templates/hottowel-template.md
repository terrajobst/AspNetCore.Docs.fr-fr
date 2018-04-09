---
uid: single-page-application/overview/templates/hottowel-template
title: Modèle de linge à chaud | Documents Microsoft
author: madskristensen
description: Modèle de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a><span data-ttu-id="4d26b-103">Modèle de linge à chaud</span><span class="sxs-lookup"><span data-stu-id="4d26b-103">Hot Towel template</span></span>
====================
<span data-ttu-id="4d26b-104">par [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="4d26b-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="4d26b-105">Le modèle MVC linge à chaud est écrits par John Papa</span><span class="sxs-lookup"><span data-stu-id="4d26b-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="4d26b-106">Choisir la version à télécharger :</span><span class="sxs-lookup"><span data-stu-id="4d26b-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="4d26b-107">Modèle MVC linge à chaud pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4d26b-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="4d26b-108">Modèle MVC linge à chaud pour Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4d26b-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="4d26b-109">Linge à chaud : Étant donné que vous ne souhaitez pas atteindre le SPA sans un !</span><span class="sxs-lookup"><span data-stu-id="4d26b-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="4d26b-110">Pour créer un SPA mais ne peut pas déterminer où commencer ?</span><span class="sxs-lookup"><span data-stu-id="4d26b-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="4d26b-111">Utilisez linge à chaud et en secondes, vous aurez un SPA et tous les outils dont vous avez besoin pour développer !</span><span class="sxs-lookup"><span data-stu-id="4d26b-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="4d26b-112">Linge à chaud crée un excellent point de départ pour la création d’une Application de Page unique (SPA) avec ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4d26b-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="4d26b-113">Prêtes à l’emploi vous il fournit une structure modulaire pour votre code de navigation de la vue, liaison de données, gestion des données riches et style simple mais élégant.</span><span class="sxs-lookup"><span data-stu-id="4d26b-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="4d26b-114">Linge à chaud offre tout ce dont vous avez besoin pour générer un SPA, afin de vous concentrer sur votre application, pas l’élément.</span><span class="sxs-lookup"><span data-stu-id="4d26b-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="4d26b-115">En savoir plus sur la création d’un SPA à partir de [de John Papa vidéos, didacticiels et les cours Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="4d26b-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="4d26b-116">Structure de l’application</span><span class="sxs-lookup"><span data-stu-id="4d26b-116">Application Structure</span></span>

<span data-ttu-id="4d26b-117">À chaud linge SPA fournit un dossier d’application qui contient les fichiers JavaScript et HTML qui définissent votre application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="4d26b-118">Dans le dossier de l’application :</span><span class="sxs-lookup"><span data-stu-id="4d26b-118">Inside the App folder:</span></span>

- <span data-ttu-id="4d26b-119">durandal</span><span class="sxs-lookup"><span data-stu-id="4d26b-119">durandal</span></span>
- <span data-ttu-id="4d26b-120">services</span><span class="sxs-lookup"><span data-stu-id="4d26b-120">services</span></span>
- <span data-ttu-id="4d26b-121">ViewModel</span><span class="sxs-lookup"><span data-stu-id="4d26b-121">viewmodels</span></span>
- <span data-ttu-id="4d26b-122">Vues</span><span class="sxs-lookup"><span data-stu-id="4d26b-122">views</span></span>

<span data-ttu-id="4d26b-123">Le dossier de l’application contient une collection de modules.</span><span class="sxs-lookup"><span data-stu-id="4d26b-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="4d26b-124">Ces modules encapsulent des fonctionnalités et déclarent des dépendances sur d’autres modules.</span><span class="sxs-lookup"><span data-stu-id="4d26b-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="4d26b-125">Le dossier views contient le code HTML pour votre application et le dossier ViewModel contient la logique de présentation pour les vues (MVVM courant).</span><span class="sxs-lookup"><span data-stu-id="4d26b-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="4d26b-126">Le dossier services est idéal pour héberger tous les services courants tels que la récupération des données HTTP ou d’une interaction de stockage local requis par votre application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="4d26b-127">Il est courant pour plusieurs ViewModel réutiliser du code à partir des modules de service.</span><span class="sxs-lookup"><span data-stu-id="4d26b-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="4d26b-128">Structure de l’Application côté serveur ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4d26b-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="4d26b-129">Linge à chaud s’appuie sur la structure ASP.NET MVC familier et puissante.</span><span class="sxs-lookup"><span data-stu-id="4d26b-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="4d26b-130">Application\_Démarrer</span><span class="sxs-lookup"><span data-stu-id="4d26b-130">App\_Start</span></span>
- <span data-ttu-id="4d26b-131">Contenu</span><span class="sxs-lookup"><span data-stu-id="4d26b-131">Content</span></span>
- <span data-ttu-id="4d26b-132">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="4d26b-132">Controllers</span></span>
- <span data-ttu-id="4d26b-133">Modèles</span><span class="sxs-lookup"><span data-stu-id="4d26b-133">Models</span></span>
- <span data-ttu-id="4d26b-134">scripts ;</span><span class="sxs-lookup"><span data-stu-id="4d26b-134">Scripts</span></span>
- <span data-ttu-id="4d26b-135">Affichages</span><span class="sxs-lookup"><span data-stu-id="4d26b-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="4d26b-136">Bibliothèques proposées</span><span class="sxs-lookup"><span data-stu-id="4d26b-136">Featured Libraries</span></span>

- <span data-ttu-id="4d26b-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4d26b-137">ASP.NET MVC</span></span>
- <span data-ttu-id="4d26b-138">API web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4d26b-138">ASP.NET Web API</span></span>
- <span data-ttu-id="4d26b-139">Optimisation du Web ASP.NET - groupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="4d26b-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="4d26b-140">[Breeze.js](http://Breezejs.com) -gestion des données riche</span><span class="sxs-lookup"><span data-stu-id="4d26b-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="4d26b-141">[Durandal.js](http://Durandaljs.com) -navigation et composition de la vue</span><span class="sxs-lookup"><span data-stu-id="4d26b-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="4d26b-142">[Knockout.js](http://Knockoutjs.com) -liaisons de données</span><span class="sxs-lookup"><span data-stu-id="4d26b-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="4d26b-143">[Require.js](http://requirejs.org) -modularité avec AMD et optimisation</span><span class="sxs-lookup"><span data-stu-id="4d26b-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="4d26b-144">[Toastr.js](http://jpapa.me/c7toastr) -messages contextuels</span><span class="sxs-lookup"><span data-stu-id="4d26b-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="4d26b-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - styles de CSS robuste</span><span class="sxs-lookup"><span data-stu-id="4d26b-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="4d26b-146">Installation via le modèle SPA linge à chaud Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4d26b-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="4d26b-147">Linge à chaud peut être installé comme un modèle Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="4d26b-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="4d26b-148">Cliquez simplement sur `File`  |  `New Project` et choisissez `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="4d26b-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="4d26b-149">Puis sélectionnez le ' Application à Page unique à chaud linge « modèle et exécuter !</span><span class="sxs-lookup"><span data-stu-id="4d26b-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="4d26b-150">Installation via le Package NuGet</span><span class="sxs-lookup"><span data-stu-id="4d26b-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="4d26b-151">Linge à chaud est également un package NuGet qui augmente d’un projet ASP.NET MVC vide existant.</span><span class="sxs-lookup"><span data-stu-id="4d26b-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="4d26b-152">Installez simplement à l’aide de Nuget, puis exécutez !</span><span class="sxs-lookup"><span data-stu-id="4d26b-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="4d26b-153">Comment créer linge à chaud ?</span><span class="sxs-lookup"><span data-stu-id="4d26b-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="4d26b-154">Démarrez simplement ajouter du code !</span><span class="sxs-lookup"><span data-stu-id="4d26b-154">Simply start adding code!</span></span>

1. <span data-ttu-id="4d26b-155">Ajoutez votre propre code côté serveur, de préférence Entity Framework et WebAPI (qui vraiment éclairer avec Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="4d26b-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="4d26b-156">Ajouter des vues à le `App/views` dossier</span><span class="sxs-lookup"><span data-stu-id="4d26b-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="4d26b-157">Ajouter ViewModel pour le `App/viewmodels` dossier</span><span class="sxs-lookup"><span data-stu-id="4d26b-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="4d26b-158">Ajouter le HTML et le masquage des liaisons de données pour vos nouveaux affichages</span><span class="sxs-lookup"><span data-stu-id="4d26b-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="4d26b-159">Mettre à jour des itinéraires de navigation `shell.js`</span><span class="sxs-lookup"><span data-stu-id="4d26b-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="4d26b-160">Procédure pas à pas du HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="4d26b-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="4d26b-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="4d26b-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="4d26b-162">index.cshtml est l’itinéraire et la vue de l’application MVC départ.</span><span class="sxs-lookup"><span data-stu-id="4d26b-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="4d26b-163">Il contient tous les balises meta standard, liens css et JavaScript les références souhaitées.</span><span class="sxs-lookup"><span data-stu-id="4d26b-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="4d26b-164">Le corps contient un seul `<div>` qui est où tout le contenu (vos vues) sera placé lorsqu’elles sont demandées.</span><span class="sxs-lookup"><span data-stu-id="4d26b-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="4d26b-165">Le `@Scripts.Render` utilise Require.js pour exécuter le point d’entrée pour le code de l’application, qui est contenu dans le `main.js` fichier.</span><span class="sxs-lookup"><span data-stu-id="4d26b-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="4d26b-166">Un écran de démarrage est fourni pour montrer comment créer un écran de démarrage pendant le charge de votre application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="4d26b-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="4d26b-167">App/main.js</span></span>

<span data-ttu-id="4d26b-168">Le `main.js` fichier contient le code qui s’exécutera dès que votre application est chargée.</span><span class="sxs-lookup"><span data-stu-id="4d26b-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="4d26b-169">Il s’agit dans lequel vous souhaitez définir votre parcours de navigation, les vues de démarrage et effectuer tout le programme d’installation/amorçage telles que l’amorçage de données de votre application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="4d26b-170">Le `main.js` fichier définit plusieurs des modules de durandal pour aider à commencer la réunion de lancement d’application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="4d26b-171">L’instruction de définition vous aide à résoudre les dépendances de modules afin qu’ils soient disponibles pour la fonction.</span><span class="sxs-lookup"><span data-stu-id="4d26b-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="4d26b-172">Tout d’abord les messages de débogage sont activées, ce qui envoient des messages sur les événements d’exécution dans la fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="4d26b-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="4d26b-173">Le code app.start indique framework durandal pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="4d26b-174">Les conventions sont définies de sorte que durandal connaît toutes les vues et ViewModel est contenus dans les mêmes dossiers nommés, respectivement.</span><span class="sxs-lookup"><span data-stu-id="4d26b-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="4d26b-175">Enfin, le `app.setRoot` intervient charges le `shell` à l’aide de prédéfini `entrance` animation.</span><span class="sxs-lookup"><span data-stu-id="4d26b-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="4d26b-176">Affichages</span><span class="sxs-lookup"><span data-stu-id="4d26b-176">Views</span></span>

<span data-ttu-id="4d26b-177">Vues sont trouvent dans le `App/views` dossier.</span><span class="sxs-lookup"><span data-stu-id="4d26b-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="4d26b-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="4d26b-178">shell.html</span></span>

<span data-ttu-id="4d26b-179">La `shell.html` contient la mise en page maître pour votre code HTML.</span><span class="sxs-lookup"><span data-stu-id="4d26b-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="4d26b-180">Toutes les autres vues seront composé quelque part dans la partie de votre `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="4d26b-181">Linge à chaud fournit un `shell` avec trois régions de ce type : un en-tête, une zone de contenu et un pied de page.</span><span class="sxs-lookup"><span data-stu-id="4d26b-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="4d26b-182">Chacune de ces régions est chargé avec contenu forme d’autres vues lorsqu’il est demandé.</span><span class="sxs-lookup"><span data-stu-id="4d26b-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="4d26b-183">Le `compose` liaisons pour l’en-tête et le pied de page sont codées en durs dans linge à chaud pour pointer vers le `nav` et `footer` consulte, respectivement.</span><span class="sxs-lookup"><span data-stu-id="4d26b-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="4d26b-184">La liaison de message pour la section `#content` est lié à la `router` élément d’actif du module.</span><span class="sxs-lookup"><span data-stu-id="4d26b-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="4d26b-185">En d’autres termes, lorsque vous cliquez sur un lien de navigation elle est vue correspondante est chargé dans ce domaine.</span><span class="sxs-lookup"><span data-stu-id="4d26b-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="4d26b-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="4d26b-186">nav.html</span></span>

<span data-ttu-id="4d26b-187">Le `nav.html` contient les liens de navigation pour SPA.</span><span class="sxs-lookup"><span data-stu-id="4d26b-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="4d26b-188">Il s’agit où la structure de menu peut être placée, par exemple.</span><span class="sxs-lookup"><span data-stu-id="4d26b-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="4d26b-189">Il s’agit souvent les données liées (à l’aide de Knockout) à la `router` module pour afficher la barre de navigation que vous avez définies dans le `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="4d26b-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="4d26b-190">Knockout recherche la liaison de données des attributs et celles qui lie la `shell` viewmodel pour afficher les itinéraires de navigation et afficher un progressbar (à l’aide d’amorçage de Twitter) si le `router` module est en cours de la navigation d’une vue à une autre (voir `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="4d26b-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="4d26b-191">Home.HTML et details.html</span><span class="sxs-lookup"><span data-stu-id="4d26b-191">home.html and details.html</span></span>

<span data-ttu-id="4d26b-192">Ces vues contiennent HTML pour des vues personnalisées.</span><span class="sxs-lookup"><span data-stu-id="4d26b-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="4d26b-193">Lorsque le `home` lien dans le `nav` clic sur le menu de la vue, le `home` affichage sera placé dans la zone de contenu de la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="4d26b-194">Ces vues peuvent être augmentées ou remplacées par vos propres affichages personnalisés.</span><span class="sxs-lookup"><span data-stu-id="4d26b-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="4d26b-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="4d26b-195">footer.html</span></span>

<span data-ttu-id="4d26b-196">Le `footer.html` contient du code HTML qui s’affiche dans le pied de page, en bas de la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="4d26b-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="4d26b-197">ViewModels</span></span>

<span data-ttu-id="4d26b-198">ViewModel se trouvent dans le `App/viewmodels` dossier.</span><span class="sxs-lookup"><span data-stu-id="4d26b-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="4d26b-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="4d26b-199">shell.js</span></span>

<span data-ttu-id="4d26b-200">Le `shell` viewmodel contient les propriétés et les fonctions qui sont liées à la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="4d26b-201">Il s’agit souvent où sont trouvent les liaisons de navigation de menu (consultez la `router.mapNav` logique).</span><span class="sxs-lookup"><span data-stu-id="4d26b-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="4d26b-202">Home.js et details.js</span><span class="sxs-lookup"><span data-stu-id="4d26b-202">home.js and details.js</span></span>

<span data-ttu-id="4d26b-203">Ces modèles ViewModel contiennent les propriétés et les fonctions qui sont liées à la `home` vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="4d26b-204">Il contient la logique de présentation de la vue et vous est le lien entre les données et la vue.</span><span class="sxs-lookup"><span data-stu-id="4d26b-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="4d26b-205">Services</span><span class="sxs-lookup"><span data-stu-id="4d26b-205">Services</span></span>

<span data-ttu-id="4d26b-206">Services sont trouvent dans le dossier services d’application.</span><span class="sxs-lookup"><span data-stu-id="4d26b-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="4d26b-207">Dans l’idéal, vos futures services comme un module dataservice, qui est responsable de la mise en route et la validation des données distantes, peut être placés.</span><span class="sxs-lookup"><span data-stu-id="4d26b-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="4d26b-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="4d26b-208">logger.js</span></span>

<span data-ttu-id="4d26b-209">Linge à chaud fournit un `logger` module dans le dossier services.</span><span class="sxs-lookup"><span data-stu-id="4d26b-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="4d26b-210">Le `logger` module est idéal pour les messages de journalisation pour la console et l’utilisateur dans la fenêtre contextuelle toasts.</span><span class="sxs-lookup"><span data-stu-id="4d26b-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
