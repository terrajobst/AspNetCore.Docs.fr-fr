---
title: Migrer à partir de ASP.NET MVC à cœur d’ASP.NET MVC
author: ardalis
description: Découvrez comment commencer la migration vers ASP.NET MVC de base d’un projet ASP.NET MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: e249be06726b307a1c41a525a132f7e0ab8b50ee
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="09e55-103">Migrer à partir de ASP.NET MVC à cœur d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="09e55-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="09e55-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="09e55-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="09e55-105">Cet article explique comment commencer la migration d’un projet ASP.NET MVC à [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="09e55-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="09e55-106">Dans le processus, il présente la plupart des éléments qui ont été modifiés dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="09e55-107">Migration à partir d’ASP.NET MVC est un processus en plusieurs étapes, et cet article couvre la configuration initiale, contrôleurs de base et les vues, contenu statique et dépendances du côté client.</span><span class="sxs-lookup"><span data-stu-id="09e55-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="09e55-108">Autres articles couvrent la migration de la configuration et le code d’identité trouvée dans de nombreux projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="09e55-109">Les numéros de version dans les exemples ne sont peut-être pas ceux en cours.</span><span class="sxs-lookup"><span data-stu-id="09e55-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="09e55-110">Vous devrez peut-être mettre à jour vos projets en conséquence.</span><span class="sxs-lookup"><span data-stu-id="09e55-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="09e55-111">Créer le projet ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="09e55-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="09e55-112">Pour illustrer la mise à niveau, nous allons commencer par la création d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="09e55-113">Créez-la avec le nom *WebApp1* pour que l’espace de noms corresponde au projet ASP.NET Core que nous créons à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="09e55-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![Boîte de dialogue Visual Studio nouveau projet](mvc/_static/new-project.png)

![Boîte de dialogue nouvelle Application Web : modèle de projet MVC sélectionné dans le panneau de configuration de modèles ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="09e55-116"> *Facultatif :* Changez le nom de la solution de *WebApp1* en *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="09e55-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="09e55-117">Visual Studio affichera le nouveau nom de la solution (*Mvc5*), ce qui permettra de distinguer plus facilement ce projet du projet suivant.</span><span class="sxs-lookup"><span data-stu-id="09e55-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="09e55-118">Créer le projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09e55-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="09e55-119">Créez une nouvelle application web ASP.NET Core *vide* avec le même nom que le projet précédent (*WebApp1*) afin de faire correspondre les espaces de noms dans les deux projets.</span><span class="sxs-lookup"><span data-stu-id="09e55-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="09e55-120">Avoir le même espace de noms rend plus facile la copie de code entre les deux projets.</span><span class="sxs-lookup"><span data-stu-id="09e55-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="09e55-121">Vous devez créer ce projet dans un autre répertoire que le projet précédent pour utiliser le même nom.</span><span class="sxs-lookup"><span data-stu-id="09e55-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Boîte de dialogue Nouveau projet](mvc/_static/new_core.png)

![Boîte de dialogue nouvelle Application Web ASP.NET : modèle de projet vide sélectionnée dans le panneau de configuration de modèles de base ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="09e55-124">*Facultatif :* créer une nouvelle application ASP.NET Core à l’aide du *Application Web* modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="09e55-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="09e55-125">Nommez le projet *WebApp1*, puis sélectionnez une option d’authentification de **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="09e55-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="09e55-126">Renommer cette application *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="09e55-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="09e55-127">Création de ce projet seront gagner du temps lors de la conversion.</span><span class="sxs-lookup"><span data-stu-id="09e55-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="09e55-128">Vous pouvez examiner le code généré de modèle pour afficher le résultat final ou copiez le code dans le projet de conversion.</span><span class="sxs-lookup"><span data-stu-id="09e55-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="09e55-129">Il est également utile lorsque vous êtes bloqué sur une étape de conversion à comparer avec le projet de modèle généré.</span><span class="sxs-lookup"><span data-stu-id="09e55-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="09e55-130">Configurer le site pour utiliser MVC</span><span class="sxs-lookup"><span data-stu-id="09e55-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="09e55-131">Installez les packages NuGet `Microsoft.AspNetCore.Mvc` et `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="09e55-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="09e55-132">`Microsoft.AspNetCore.Mvc` est le framework ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="09e55-133">`Microsoft.AspNetCore.StaticFiles` est le Gestionnaire de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="09e55-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="09e55-134">Le runtime ASP.NET est modulaire, et vous devez explicitement s’abonner à des fichiers statiques (consultez [travailler avec des fichiers statiques](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="09e55-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Work with static files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="09e55-135">Ouvrez le *.csproj* fichier (avec le bouton droit dans le projet de **l’Explorateur de solutions** et sélectionnez **WebApp1.csproj de modifier**) et ajoutez une cible `PrepareForPublish` :</span><span class="sxs-lookup"><span data-stu-id="09e55-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="09e55-136">La cible `PrepareForPublish` est nécessaire pour l’acquisition des bibliothèques côté client via Bower.</span><span class="sxs-lookup"><span data-stu-id="09e55-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="09e55-137">Nous en parlerons plus tard.</span><span class="sxs-lookup"><span data-stu-id="09e55-137">We'll talk about that later.</span></span>

* <span data-ttu-id="09e55-138">Ouvrez le fichier *Startup.cs* et modifiez le code comme suit :</span><span class="sxs-lookup"><span data-stu-id="09e55-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="09e55-139">La méthode d’extension `UseStaticFiles` ajoute le Gestionnaire de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="09e55-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="09e55-140">Comme mentionné précédemment, le runtime ASP.NET est modulaire, et vous devez explicitement autoriser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="09e55-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="09e55-141">La méthode d’extension `UseMvc` ajoute le routage.</span><span class="sxs-lookup"><span data-stu-id="09e55-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="09e55-142">Pour plus d’informations, consultez [démarrage de l’Application](../fundamentals/startup.md) et [routage](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="09e55-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="09e55-143">Ajouter un contrôleur et une vue</span><span class="sxs-lookup"><span data-stu-id="09e55-143">Add a controller and view</span></span>

<span data-ttu-id="09e55-144">Dans cette section, vous allez ajouter un contrôleur minimal et la vue comme des espaces réservés pour le contrôleur ASP.NET MVC et les vues que vous allez migrer dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="09e55-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="09e55-145">Ajoutez un dossier *contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="09e55-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="09e55-146">Ajoutez une **classe de contrôleur MVC** portant le nom *HomeController.cs* au dossier *contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="09e55-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="09e55-148">Ajoutez un dossier *Views*.</span><span class="sxs-lookup"><span data-stu-id="09e55-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="09e55-149">Ajoutez un dossier *Views/Home*.</span><span class="sxs-lookup"><span data-stu-id="09e55-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="09e55-150">Ajoutez une page de vue MVC *Index.cshtml* au dossier *Views/Home*.</span><span class="sxs-lookup"><span data-stu-id="09e55-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/view.png)

<span data-ttu-id="09e55-152">La structure de projet est indiquée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="09e55-152">The project structure is shown below:</span></span>

![Explorateur de solutions affichant les fichiers et dossiers de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="09e55-154">Remplacez le contenu du fichier *Views/Home/Index.cshtml* avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09e55-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="09e55-155">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="09e55-155">Run the app.</span></span>

![Application Web ouverte dans Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="09e55-157">Consultez [contrôleurs](xref:mvc/controllers/actions) et [vues](xref:mvc/views/overview) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="09e55-157">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="09e55-158">Maintenant que nous avons un projet ASP.NET Core minimal, nous pouvons commencer la migration des fonctionnalités à partir du projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="09e55-159">Vous devrez déplacer les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="09e55-159">We will need to move the following:</span></span>

* <span data-ttu-id="09e55-160">Contenu côté client (CSS, polices et scripts)</span><span class="sxs-lookup"><span data-stu-id="09e55-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="09e55-161">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="09e55-161">controllers</span></span>

* <span data-ttu-id="09e55-162">Vues</span><span class="sxs-lookup"><span data-stu-id="09e55-162">views</span></span>

* <span data-ttu-id="09e55-163">Modèles</span><span class="sxs-lookup"><span data-stu-id="09e55-163">models</span></span>

* <span data-ttu-id="09e55-164">Regroupement</span><span class="sxs-lookup"><span data-stu-id="09e55-164">bundling</span></span>

* <span data-ttu-id="09e55-165">Filtres</span><span class="sxs-lookup"><span data-stu-id="09e55-165">filters</span></span>

* <span data-ttu-id="09e55-166">Connexion/déconnexion, identité (ceci sera effectué dans le prochain didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="09e55-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="09e55-167">Contrôleurs et vues</span><span class="sxs-lookup"><span data-stu-id="09e55-167">Controllers and views</span></span>

* <span data-ttu-id="09e55-168">Chacune des méthodes copier à partir de l’ASP.NET MVC `HomeController` au nouveau `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="09e55-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="09e55-169">Notez que dans ASP.NET MVC, type de retour de méthode de modèle prédéfini contrôleur action est [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); dans ASP.NET MVC de base, les méthodes d’action retournés `IActionResult` à la place.</span><span class="sxs-lookup"><span data-stu-id="09e55-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="09e55-170">`ActionResult` implémente `IActionResult`, il est donc inutile de modifier le type de retour de vos méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="09e55-170">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="09e55-171">Copiez les fichiers de vues Razor *About.cshtml*, *Contact.cshtml* et *Index.cshtml* du projet ASP.NET MVC dans le projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="09e55-172">Exécutez l’application ASP.NET Core et chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="09e55-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="09e55-173">Nous n’avons pas encore effectué la migration du fichier de mise en page ou des styles, donc les vues de rendu ne contiennent que le contenu des fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="09e55-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="09e55-174">Comme vous ne disposez pas des liens générés du fichier de mise en page pour les vues `About` et `Contact`, vous devez les appeler à partir du navigateur (remplacez **4492** par le numéro de port utilisé dans votre projet).</span><span class="sxs-lookup"><span data-stu-id="09e55-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Page de contact](mvc/_static/contact-page.png)

<span data-ttu-id="09e55-176">Notez l’absence de styles et d’éléments de menu.</span><span class="sxs-lookup"><span data-stu-id="09e55-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="09e55-177">Nous le corrigerons dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="09e55-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="09e55-178">Contenu statique</span><span class="sxs-lookup"><span data-stu-id="09e55-178">Static content</span></span>

<span data-ttu-id="09e55-179">Dans les versions précédentes d’ASP.NET MVC, le contenu statique était hébergé à partir de la racine du projet web et mélangé avec les fichiers côté serveur.</span><span class="sxs-lookup"><span data-stu-id="09e55-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="09e55-180">Dans ASP.NET Core, le contenu statique est hébergé dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="09e55-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="09e55-181">Vous devez copier le contenu statique de votre ancienne application ASP.NET MVC dans le dossier *wwwroot* de votre projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="09e55-182">Dans cette exemple de conversion :</span><span class="sxs-lookup"><span data-stu-id="09e55-182">In this sample conversion:</span></span>

* <span data-ttu-id="09e55-183">Copie le *favicon.ico* fichier à partir de l’ancien projet MVC dans le *wwwroot* dossier dans le projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="09e55-184">L’ancien ASP.NET MVC project utilise [Bootstrap](http://getbootstrap.com/) pour que son style et le programme d’amorçage des fichiers dans le *contenu* et *Scripts* dossiers.</span><span class="sxs-lookup"><span data-stu-id="09e55-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="09e55-185">Le modèle, ce qui a généré l’ancien projet ASP.NET MVC, fait référence à l’amorçage dans le fichier de disposition (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="09e55-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="09e55-186">Vous pouvez copier le *bootstrap.js* et *bootstrap.css* à partir de l’ASP.NET MVC, les fichiers de projet pour le *wwwroot* n’utilise pas de dossier dans le nouveau projet, mais cette approche le mécanisme améliorée pour la gestion des dépendances de côté client dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="09e55-187">Dans le nouveau projet, nous allons ajouter la prise en charge de Bootstrap (et d’autres bibliothèques côté client) à l’aide de [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="09e55-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="09e55-188">Ajoutez un fichier de configuration [Bower](https://bower.io/) nommé *bower.json* à la racine du projet (cliquez avec le bouton droit sur le projet, puis sur **AAjouter > Nouvel élément > Fichier de Configuration Bower**).</span><span class="sxs-lookup"><span data-stu-id="09e55-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="09e55-189">Ajoutez [Bootstrap](http://getbootstrap.com/) et [jQuery](https://jquery.com/) au fichier (voir les lignes en surbrillance ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="09e55-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="09e55-190">Lors de l’enregistrement du fichier, Bower télécharge automatiquement les dépendances dans le dossier *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="09e55-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="09e55-191">Vous pouvez utiliser la **zone de recherche dans l’Explorateur de solutions** pour rechercher le chemin des ressources :</span><span class="sxs-lookup"><span data-stu-id="09e55-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![ressources de jQuery apparaissent dans les résultats de recherche de l’Explorateur de solutions](mvc/_static/search.png)

<span data-ttu-id="09e55-193">Consultez [Pour plus d’informations, consultez Gérer les packages côté client avec Bower](../client-side/bower.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="09e55-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="09e55-194">Migrer le fichier de disposition</span><span class="sxs-lookup"><span data-stu-id="09e55-194">Migrate the layout file</span></span>

* <span data-ttu-id="09e55-195">Copiez le fichier*_ViewStart.cshtml* à partir du dossier *Views* de l’ancien projet ASP.NET MVC dans le dossier *Views* du projet de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="09e55-196">Le fichier *_ViewStart.cshtml* n’a pas changé dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="09e55-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="09e55-197">Créez un dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="09e55-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="09e55-198">*Facultatif :* copie *_ViewImports.cshtml* à partir de la *FullAspNetCore* du projet MVC *vues* dossier dans le projet de ASP.NET Core  *Vues* dossier.</span><span class="sxs-lookup"><span data-stu-id="09e55-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="09e55-199">Supprimez toute déclaration d’espace de noms dans le *_ViewImports.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="09e55-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="09e55-200">Le *_ViewImports.cshtml* fournit des espaces de noms pour tous les fichiers de l’affichage de fichiers et permet de bénéficier de [programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="09e55-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="09e55-201">Programmes d’assistance de balise sont utilisés dans le nouveau fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="09e55-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="09e55-202">Le *_ViewImports.cshtml* fichier est une nouveauté pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="09e55-203">Copiez le fichier *_Layout.cshtml* à partir du dossier *Views/Shared* de l’ancien projet ASP.NET MVC dans le dossier *Views/Shared* du projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09e55-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="09e55-204">Ouvrez *_Layout.cshtml* et apportez les modifications suivantes (le code complet est indiqué ci-dessous) :</span><span class="sxs-lookup"><span data-stu-id="09e55-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="09e55-205">Remplacez `@Styles.Render("~/Content/css")` par un élément `<link>` pour charger *bootstrap.css* (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="09e55-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="09e55-206">Supprimez `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="09e55-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="09e55-207">Commentez la ligne `@Html.Partial("_LoginPartial")` (en la plaçant entre `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="09e55-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="09e55-208">Nous y reviendrons dans un prochain didacticiel.</span><span class="sxs-lookup"><span data-stu-id="09e55-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="09e55-209">Remplacez `@Scripts.Render("~/bundles/jquery")` par un élément `<script>` (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="09e55-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="09e55-210">Remplacez `@Scripts.Render("~/bundles/bootstrap")` par un élément `<script>` (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="09e55-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="09e55-211">Le lien CSS de remplacement :</span><span class="sxs-lookup"><span data-stu-id="09e55-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="09e55-212">Les balises de script de remplacement :</span><span class="sxs-lookup"><span data-stu-id="09e55-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="09e55-213">La mise à jour *_Layout.cshtml* fichier est présenté ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="09e55-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="09e55-214">Afficher le site dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="09e55-214">View the site in the browser.</span></span> <span data-ttu-id="09e55-215">Il doit maintenant charger correctement, avec les styles attendus en place.</span><span class="sxs-lookup"><span data-stu-id="09e55-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="09e55-216">*Facultatif :* Vous pouvez essayer d’utiliser le nouveau fichier de mise en page.</span><span class="sxs-lookup"><span data-stu-id="09e55-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="09e55-217">Pour ce projet, copiez le fichier de mise en page à partir du projet *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="09e55-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="09e55-218">Le nouveau fichier de mise en page utilise les [Tags Helpers](xref:mvc/views/tag-helpers/intro) et comprend d’autres améliorations.</span><span class="sxs-lookup"><span data-stu-id="09e55-218">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="09e55-219">Configurer le regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="09e55-219">Configure bundling and minification</span></span>

<span data-ttu-id="09e55-220">Pour plus d’informations sur la configuration du regroupement et de la minimisation, consultez [Regroupement et Minimisation](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="09e55-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="09e55-221">Résolution des erreurs HTTP 500</span><span class="sxs-lookup"><span data-stu-id="09e55-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="09e55-222">De nombreux problèmes peuvent provoquer un message d’erreur HTTP 500 qui ne donne aucune information sur la source du problème.</span><span class="sxs-lookup"><span data-stu-id="09e55-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="09e55-223">Par exemple, si le fichier *Views/_ViewImports.cshtml* contient un espace de noms qui n’existe pas dans votre projet, vous obtenez une erreur HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="09e55-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="09e55-224">Pour obtenir un message d’erreur détaillé, ajoutez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="09e55-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="09e55-225">Consultez **à l’aide de la Page d’Exception Developer** dans [gérer les erreurs](../fundamentals/error-handling.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="09e55-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09e55-226">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="09e55-226">Additional resources</span></span>

* [<span data-ttu-id="09e55-227">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="09e55-227">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="09e55-228">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="09e55-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
