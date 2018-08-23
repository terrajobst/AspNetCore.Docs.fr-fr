---
uid: single-page-application/overview/templates/hottowel-template
title: Modèle de serviette à chaud | Microsoft Docs
author: madskristensen
description: Modèle de HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834646"
---
<a name="hot-towel-template"></a><span data-ttu-id="79f4d-103">Modèle hot Towel</span><span class="sxs-lookup"><span data-stu-id="79f4d-103">Hot Towel template</span></span>
====================
<span data-ttu-id="79f4d-104">par [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="79f4d-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="79f4d-105">Le modèle MVC SERVIETTE à chaud est écrite par John Papa</span><span class="sxs-lookup"><span data-stu-id="79f4d-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="79f4d-106">Choisir la version à télécharger :</span><span class="sxs-lookup"><span data-stu-id="79f4d-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="79f4d-107">Modèle MVC hot Towel pour Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="79f4d-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="79f4d-108">Modèle MVC hot Towel pour Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="79f4d-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="79f4d-109">Hot Towel : Étant donné que vous ne souhaitez pas accéder à l’application SPA sans un !</span><span class="sxs-lookup"><span data-stu-id="79f4d-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="79f4d-110">À générer une application à page unique, mais ne peut pas décider où commencer ?</span><span class="sxs-lookup"><span data-stu-id="79f4d-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="79f4d-111">Utilisez Hot Towel et en quelques secondes, vous aurez une application SPA et tous les outils que nécessaires à la création dessus !</span><span class="sxs-lookup"><span data-stu-id="79f4d-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="79f4d-112">Hot Towel crée un excellent point de départ pour la création d’une Application à Page unique (SPA) avec ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79f4d-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="79f4d-113">Prêt à l’emploi vous il fournit une structure modulaire pour votre code navigation de la vue, liaison de données, gestion des données riches et styles simples mais élégantes.</span><span class="sxs-lookup"><span data-stu-id="79f4d-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="79f4d-114">Hot Towel fournit tout ce dont vous avez besoin pour générer une application SPA, afin de vous concentrer sur votre application, pas la plomberie.</span><span class="sxs-lookup"><span data-stu-id="79f4d-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="79f4d-115">En savoir plus sur la création d’une application à page unique à partir de [vidéos, des didacticiels et des cours Pluralsight John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="79f4d-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="79f4d-116">Structure de l’application</span><span class="sxs-lookup"><span data-stu-id="79f4d-116">Application Structure</span></span>

<span data-ttu-id="79f4d-117">Hot Towel SPA fournit un dossier d’application qui contient les fichiers JavaScript et HTML qui définissent votre application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="79f4d-118">Dans le dossier d’application :</span><span class="sxs-lookup"><span data-stu-id="79f4d-118">Inside the App folder:</span></span>

- <span data-ttu-id="79f4d-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="79f4d-119">durandal</span></span>
- <span data-ttu-id="79f4d-120">services</span><span class="sxs-lookup"><span data-stu-id="79f4d-120">services</span></span>
- <span data-ttu-id="79f4d-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="79f4d-121">viewmodels</span></span>
- <span data-ttu-id="79f4d-122">vues</span><span class="sxs-lookup"><span data-stu-id="79f4d-122">views</span></span>

<span data-ttu-id="79f4d-123">Le dossier de l’application contient une collection de modules.</span><span class="sxs-lookup"><span data-stu-id="79f4d-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="79f4d-124">Ces modules encapsulent des fonctionnalités et déclarent des dépendances sur d’autres modules.</span><span class="sxs-lookup"><span data-stu-id="79f4d-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="79f4d-125">Le dossier views contient le code HTML de votre application et le dossier viewmodels contient la logique de présentation pour les vues (MVVM courant).</span><span class="sxs-lookup"><span data-stu-id="79f4d-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="79f4d-126">Le dossier services est idéal pour héberger tous les services courants tels que la récupération des données HTTP ou d’une interaction de stockage local requis par votre application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="79f4d-127">Il est courant pour plusieurs ViewModel à réutiliser le code à partir des modules de service.</span><span class="sxs-lookup"><span data-stu-id="79f4d-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="79f4d-128">Structure d’Application ASP.NET MVC serveur côté</span><span class="sxs-lookup"><span data-stu-id="79f4d-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="79f4d-129">Hot Towel s’appuie sur la structure ASP.NET MVC familière et puissante.</span><span class="sxs-lookup"><span data-stu-id="79f4d-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="79f4d-130">Application\_Démarrer</span><span class="sxs-lookup"><span data-stu-id="79f4d-130">App\_Start</span></span>
- <span data-ttu-id="79f4d-131">Contenu</span><span class="sxs-lookup"><span data-stu-id="79f4d-131">Content</span></span>
- <span data-ttu-id="79f4d-132">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="79f4d-132">Controllers</span></span>
- <span data-ttu-id="79f4d-133">Modèles</span><span class="sxs-lookup"><span data-stu-id="79f4d-133">Models</span></span>
- <span data-ttu-id="79f4d-134">scripts ;</span><span class="sxs-lookup"><span data-stu-id="79f4d-134">Scripts</span></span>
- <span data-ttu-id="79f4d-135">Affichages</span><span class="sxs-lookup"><span data-stu-id="79f4d-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="79f4d-136">Bibliothèques par défaut</span><span class="sxs-lookup"><span data-stu-id="79f4d-136">Featured Libraries</span></span>

- <span data-ttu-id="79f4d-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="79f4d-137">ASP.NET MVC</span></span>
- <span data-ttu-id="79f4d-138">API web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="79f4d-138">ASP.NET Web API</span></span>
- <span data-ttu-id="79f4d-139">L’optimisation Web ASP.NET - regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="79f4d-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="79f4d-140">[Breeze.js](http://Breezejs.com) -gestion des données riche</span><span class="sxs-lookup"><span data-stu-id="79f4d-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="79f4d-141">[Durandal.js](http://Durandaljs.com) -navigation et la composition de la vue</span><span class="sxs-lookup"><span data-stu-id="79f4d-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="79f4d-142">[Knockout.js](http://Knockoutjs.com) -des liaisons de données</span><span class="sxs-lookup"><span data-stu-id="79f4d-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="79f4d-143">[Require.js](http://requirejs.org) -modularité avec AMD et optimisation</span><span class="sxs-lookup"><span data-stu-id="79f4d-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="79f4d-144">[Toastr.js](http://jpapa.me/c7toastr) -messages contextuels</span><span class="sxs-lookup"><span data-stu-id="79f4d-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="79f4d-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) : styles de CSS robuste</span><span class="sxs-lookup"><span data-stu-id="79f4d-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="79f4d-146">Installation via le modèle SPA Hot Towel Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="79f4d-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="79f4d-147">Hot Towel peut être installé comme un modèle Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="79f4d-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="79f4d-148">Cliquez simplement sur `File`  |  `New Project` et choisissez `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="79f4d-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="79f4d-149">Puis sélectionnez le « Hot Towel Single Page Application « modèle et exécution !</span><span class="sxs-lookup"><span data-stu-id="79f4d-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="79f4d-150">Installation via le Package NuGet</span><span class="sxs-lookup"><span data-stu-id="79f4d-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="79f4d-151">Hot Towel est également un package NuGet qui augmente d’un projet ASP.NET MVC vide existant.</span><span class="sxs-lookup"><span data-stu-id="79f4d-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="79f4d-152">Installez simplement à l’aide de Nuget, puis exécutez !</span><span class="sxs-lookup"><span data-stu-id="79f4d-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="79f4d-153">Comment créer SERVIETTE chaud ?</span><span class="sxs-lookup"><span data-stu-id="79f4d-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="79f4d-154">Démarrez simplement l’ajout de code !</span><span class="sxs-lookup"><span data-stu-id="79f4d-154">Simply start adding code!</span></span>

1. <span data-ttu-id="79f4d-155">Ajouter votre propre code côté serveur, de préférence Entity Framework et API Web (qui se distinguent avec Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="79f4d-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="79f4d-156">Ajouter des affichages pour le `App/views` dossier</span><span class="sxs-lookup"><span data-stu-id="79f4d-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="79f4d-157">Ajouter les viewmodels à la `App/viewmodels` dossier</span><span class="sxs-lookup"><span data-stu-id="79f4d-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="79f4d-158">Ajouter des liaisons de données HTML et Knockout à vos nouvelles vues</span><span class="sxs-lookup"><span data-stu-id="79f4d-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="79f4d-159">Mettre à jour les itinéraires de la navigation dans `shell.js`</span><span class="sxs-lookup"><span data-stu-id="79f4d-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="79f4d-160">Procédure pas à pas du code HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="79f4d-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="79f4d-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="79f4d-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="79f4d-162">index.cshtml est l’itinéraire et la vue de l’application MVC départ.</span><span class="sxs-lookup"><span data-stu-id="79f4d-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="79f4d-163">Il contient toutes les balises meta standard, les liens de css et les références JavaScript que vous pouvez l’imaginer.</span><span class="sxs-lookup"><span data-stu-id="79f4d-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="79f4d-164">Le corps contient un seul `<div>` qui est tout le contenu (vos vues) emplacement quand ils sont demandés.</span><span class="sxs-lookup"><span data-stu-id="79f4d-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="79f4d-165">Le `@Scripts.Render` utilise Require.js pour exécuter le point d’entrée pour le code de l’application, qui est contenu dans le `main.js` fichier.</span><span class="sxs-lookup"><span data-stu-id="79f4d-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="79f4d-166">Un écran de démarrage est fourni pour montrer comment créer un écran de démarrage pendant le chargement de votre application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="79f4d-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="79f4d-167">App/main.js</span></span>

<span data-ttu-id="79f4d-168">Le `main.js` fichier contient le code qui s’exécutera dès que votre application est chargée.</span><span class="sxs-lookup"><span data-stu-id="79f4d-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="79f4d-169">Il s’agit dans lequel vous souhaitez définir vos routes de navigation, vos vues de démarrage et effectuer tout le programme d’installation/amorçage telles que l’amorçage des données de votre application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="79f4d-170">Le `main.js` fichier définit plusieurs des modules de durandal pour aider le lancement de l’application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="79f4d-171">L’instruction de définition vous aide à résoudre les dépendances de modules afin qu’ils soient disponibles pour la fonction.</span><span class="sxs-lookup"><span data-stu-id="79f4d-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="79f4d-172">Tout d’abord les messages de débogage sont activées, ce qui envoient des messages sur les événements d’exécution dans la fenêtre de console.</span><span class="sxs-lookup"><span data-stu-id="79f4d-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="79f4d-173">Le code app.start indique au framework durandal pour démarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="79f4d-174">Les conventions sont définies afin que durandal connaît toutes les vues et viewmodels sont contenus dans les mêmes dossiers nommés, respectivement.</span><span class="sxs-lookup"><span data-stu-id="79f4d-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="79f4d-175">Enfin, le `app.setRoot` intervient charges le `shell` à l’aide de prédéfini `entrance` animation.</span><span class="sxs-lookup"><span data-stu-id="79f4d-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="79f4d-176">Affichages</span><span class="sxs-lookup"><span data-stu-id="79f4d-176">Views</span></span>

<span data-ttu-id="79f4d-177">Vues sont trouvent dans le `App/views` dossier.</span><span class="sxs-lookup"><span data-stu-id="79f4d-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="79f4d-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="79f4d-178">shell.html</span></span>

<span data-ttu-id="79f4d-179">Le `shell.html` contient la disposition principale pour votre code HTML.</span><span class="sxs-lookup"><span data-stu-id="79f4d-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="79f4d-180">Toutes vos autres vues se composera quelque part dans le côté de votre `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="79f4d-181">Hot Towel fournit un `shell` avec trois régions de ce type : un en-tête, une zone de contenu et un pied de page.</span><span class="sxs-lookup"><span data-stu-id="79f4d-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="79f4d-182">Chacune de ces régions est chargé avec contenu forme d’autres vues lors de la demande.</span><span class="sxs-lookup"><span data-stu-id="79f4d-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="79f4d-183">Le `compose` liaisons pour l’en-tête et le pied de page sont figés dans Hot Towel pour pointer vers le `nav` et `footer` consulte, respectivement.</span><span class="sxs-lookup"><span data-stu-id="79f4d-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="79f4d-184">La liaison de compose pour la section `#content` est lié à la `router` élément actif du module.</span><span class="sxs-lookup"><span data-stu-id="79f4d-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="79f4d-185">En d’autres termes, lorsque vous cliquez sur un lien de navigation est vue correspondante est chargé dans ce domaine.</span><span class="sxs-lookup"><span data-stu-id="79f4d-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="79f4d-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="79f4d-186">nav.html</span></span>

<span data-ttu-id="79f4d-187">Le `nav.html` contient des liens de navigation pour l’application SPA.</span><span class="sxs-lookup"><span data-stu-id="79f4d-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="79f4d-188">Il s’agit où la structure de menu peut être placée, par exemple.</span><span class="sxs-lookup"><span data-stu-id="79f4d-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="79f4d-189">Il s’agit souvent les données liées (à l’aide de Knockout) à la `router` module pour afficher le volet de navigation vous avez défini dans le `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="79f4d-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="79f4d-190">Knockout recherche la liaison de données des attributs et celles qui lie la `shell` viewmodel pour afficher les itinéraires de navigation et afficher un progressbar (à l’aide de Twitter Bootstrap) si le `router` module est en train de naviguer d’une vue à une autre (voir `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="79f4d-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="79f4d-191">Home.HTML et details.html</span><span class="sxs-lookup"><span data-stu-id="79f4d-191">home.html and details.html</span></span>

<span data-ttu-id="79f4d-192">Ces vues contiennent HTML pour des vues personnalisées.</span><span class="sxs-lookup"><span data-stu-id="79f4d-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="79f4d-193">Lorsque le `home` lien dans le `nav` clic sur le menu de la vue, le `home` affichage sera placé dans la zone de contenu de la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="79f4d-194">Ces vues peuvent être augmentées ou remplacés par vos propres affichages personnalisés.</span><span class="sxs-lookup"><span data-stu-id="79f4d-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="79f4d-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="79f4d-195">footer.html</span></span>

<span data-ttu-id="79f4d-196">Le `footer.html` contient du code HTML qui s’affiche dans le pied de page, en bas de la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="79f4d-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="79f4d-197">ViewModels</span></span>

<span data-ttu-id="79f4d-198">ViewModels se trouvent dans le `App/viewmodels` dossier.</span><span class="sxs-lookup"><span data-stu-id="79f4d-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="79f4d-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="79f4d-199">shell.js</span></span>

<span data-ttu-id="79f4d-200">Le `shell` viewmodel contient les propriétés et les fonctions qui sont liées à la `shell` vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="79f4d-201">Il s’agit souvent où sont trouvent les liaisons de navigation de menu (voir la `router.mapNav` logique).</span><span class="sxs-lookup"><span data-stu-id="79f4d-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="79f4d-202">Home.js et details.js</span><span class="sxs-lookup"><span data-stu-id="79f4d-202">home.js and details.js</span></span>

<span data-ttu-id="79f4d-203">Ces viewmodels contiennent les propriétés et les fonctions qui sont liées à la `home` vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="79f4d-204">Il contient la logique de présentation de la vue et vous est le lien entre les données et la vue.</span><span class="sxs-lookup"><span data-stu-id="79f4d-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="79f4d-205">Services</span><span class="sxs-lookup"><span data-stu-id="79f4d-205">Services</span></span>

<span data-ttu-id="79f4d-206">Services sont trouvent dans le dossier/services d’application.</span><span class="sxs-lookup"><span data-stu-id="79f4d-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="79f4d-207">Dans l’idéal, vos futurs services tel qu’un module dataservice, qui est responsable de l’obtention et la validation des données à distance, peut être placées.</span><span class="sxs-lookup"><span data-stu-id="79f4d-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="79f4d-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="79f4d-208">logger.js</span></span>

<span data-ttu-id="79f4d-209">Hot Towel fournit un `logger` module dans le dossier services.</span><span class="sxs-lookup"><span data-stu-id="79f4d-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="79f4d-210">Le `logger` module est idéal pour les messages de journalisation dans la console et à l’utilisateur dans la fenêtre contextuelle toasts.</span><span class="sxs-lookup"><span data-stu-id="79f4d-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
