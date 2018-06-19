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
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851025"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="10f5c-103">Migrer à partir de ASP.NET MVC à cœur d’ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="10f5c-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="10f5c-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), et [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="10f5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="10f5c-105">Cet article explique comment commencer la migration d’un projet ASP.NET MVC à [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="10f5c-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="10f5c-106">Dans le processus, il présente la plupart des éléments qui ont été modifiés dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="10f5c-107">Migration à partir d’ASP.NET MVC est un processus en plusieurs étapes, et cet article couvre la configuration initiale, contrôleurs de base et les vues, contenu statique et dépendances du côté client.</span><span class="sxs-lookup"><span data-stu-id="10f5c-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="10f5c-108">Autres articles couvrent la migration de la configuration et le code d’identité trouvée dans de nombreux projets ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="10f5c-109">Les numéros de version dans les exemples ne sont peut-être pas ceux en cours.</span><span class="sxs-lookup"><span data-stu-id="10f5c-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="10f5c-110">Vous devrez peut-être mettre à jour vos projets en conséquence.</span><span class="sxs-lookup"><span data-stu-id="10f5c-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="10f5c-111">Créer le projet ASP.NET MVC starter</span><span class="sxs-lookup"><span data-stu-id="10f5c-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="10f5c-112">Pour illustrer la mise à niveau, nous allons commencer par la création d’une application ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="10f5c-113">Créer avec le nom *WebApp1* afin de l’espace de noms correspond au projet ASP.NET Core nous créer à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="10f5c-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![Boîte de dialogue Visual Studio nouveau projet](mvc/_static/new-project.png)

![Boîte de dialogue nouvelle Application Web : modèle de projet MVC sélectionné dans le panneau de configuration de modèles ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="10f5c-116">*Facultatif :* Changez le nom de la solution de *WebApp1* en *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="10f5c-117">Visual Studio affiche le nouveau nom de solution (*Mvc5*), ce qui facilite l’indiquer à ce projet à partir du projet suivant.</span><span class="sxs-lookup"><span data-stu-id="10f5c-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="10f5c-118">Créer le projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="10f5c-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="10f5c-119">Créez une nouvelle application web ASP.NET Core *vide* avec le même nom que le projet précédent (*WebApp1*) afin de faire correspondre les espaces de noms dans les deux projets.</span><span class="sxs-lookup"><span data-stu-id="10f5c-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="10f5c-120">Avoir le même espace de noms rend plus facile la copie de code entre les deux projets.</span><span class="sxs-lookup"><span data-stu-id="10f5c-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="10f5c-121">Vous devez créer ce projet dans un autre répertoire que le projet précédent pour utiliser le même nom.</span><span class="sxs-lookup"><span data-stu-id="10f5c-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Boîte de dialogue Nouveau projet](mvc/_static/new_core.png)

![Boîte de dialogue nouvelle Application Web ASP.NET : modèle de projet vide sélectionnée dans le panneau de configuration de modèles de base ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="10f5c-124">*Facultatif :* créer une nouvelle application ASP.NET Core à l’aide du *Application Web* modèle de projet.</span><span class="sxs-lookup"><span data-stu-id="10f5c-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="10f5c-125">Nommez le projet *WebApp1*, puis sélectionnez une option d’authentification de **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="10f5c-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="10f5c-126">Renommer cette application *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="10f5c-127">Création de projet pouvez gagner du temps lors de la conversion.</span><span class="sxs-lookup"><span data-stu-id="10f5c-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="10f5c-128">Vous pouvez examiner le code généré de modèle pour afficher le résultat final ou copiez le code dans le projet de conversion.</span><span class="sxs-lookup"><span data-stu-id="10f5c-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="10f5c-129">Il est également utile lorsque vous êtes bloqué sur une étape de conversion à comparer avec le projet de modèle généré.</span><span class="sxs-lookup"><span data-stu-id="10f5c-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="10f5c-130">Configurer le site pour utiliser MVC</span><span class="sxs-lookup"><span data-stu-id="10f5c-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="10f5c-131">Lorsque vous ciblez .NET Core, la metapackage ASP.NET Core est ajouté au projet, appelé `Microsoft.AspNetCore.All` par défaut.</span><span class="sxs-lookup"><span data-stu-id="10f5c-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="10f5c-132">Ce package contient des packages comme `Microsoft.AspNetCore.Mvc` et `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="10f5c-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="10f5c-133">Si vous ciblez .NET Framework, les références de package doivent être répertoriée individuellement dans le fichier \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="10f5c-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="10f5c-134">`Microsoft.AspNetCore.Mvc` est le framework ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="10f5c-135">`Microsoft.AspNetCore.StaticFiles` est le Gestionnaire de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="10f5c-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="10f5c-136">Le runtime ASP.NET Core est modulaire, et vous devez explicitement s’abonner à des fichiers statiques (consultez [fichiers statiques](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="10f5c-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="10f5c-137">Ouvrez le fichier *Startup.cs* et modifiez le code comme suit :</span><span class="sxs-lookup"><span data-stu-id="10f5c-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="10f5c-138">La méthode d’extension `UseStaticFiles` ajoute le Gestionnaire de fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="10f5c-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="10f5c-139">Comme mentionné précédemment, le runtime ASP.NET est modulaire, et vous devez explicitement autoriser les fichiers statiques.</span><span class="sxs-lookup"><span data-stu-id="10f5c-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="10f5c-140">La méthode d’extension `UseMvc` ajoute le routage.</span><span class="sxs-lookup"><span data-stu-id="10f5c-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="10f5c-141">Pour plus d’informations, consultez [démarrage de l’Application](xref:fundamentals/startup) et [routage](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="10f5c-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="10f5c-142">Ajouter un contrôleur et une vue</span><span class="sxs-lookup"><span data-stu-id="10f5c-142">Add a controller and view</span></span>

<span data-ttu-id="10f5c-143">Dans cette section, vous allez ajouter un contrôleur minimal et la vue comme des espaces réservés pour le contrôleur ASP.NET MVC et les vues que vous allez migrer dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="10f5c-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="10f5c-144">Ajoutez un dossier *contrôleurs*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="10f5c-145">Ajouter un **classe de contrôleur** nommé *HomeController.cs* à la *contrôleurs* dossier.</span><span class="sxs-lookup"><span data-stu-id="10f5c-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="10f5c-147">Ajoutez un dossier *Views*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="10f5c-148">Ajoutez un dossier *Views/Home*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="10f5c-149">Ajouter un **vue Razor** nommé *Index.cshtml* à la *Views/Home* dossier.</span><span class="sxs-lookup"><span data-stu-id="10f5c-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Boîte de dialogue Ajouter un nouvel élément](mvc/_static/view.png)

<span data-ttu-id="10f5c-151">La structure de projet est indiquée ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="10f5c-151">The project structure is shown below:</span></span>

![Explorateur de solutions affichant les fichiers et dossiers de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="10f5c-153">Remplacez le contenu du fichier *Views/Home/Index.cshtml* avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="10f5c-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="10f5c-154">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="10f5c-154">Run the app.</span></span>

![Application Web ouverte dans Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="10f5c-156">Consultez [contrôleurs](xref:mvc/controllers/actions) et [vues](xref:mvc/views/overview) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="10f5c-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="10f5c-157">Maintenant que nous avons un projet ASP.NET Core minimal, nous pouvons commencer la migration des fonctionnalités à partir du projet ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="10f5c-158">Nous devons déplacer les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="10f5c-158">We need to move the following:</span></span>

* <span data-ttu-id="10f5c-159">Contenu côté client (CSS, polices et scripts)</span><span class="sxs-lookup"><span data-stu-id="10f5c-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="10f5c-160">Contrôleurs</span><span class="sxs-lookup"><span data-stu-id="10f5c-160">controllers</span></span>

* <span data-ttu-id="10f5c-161">Vues</span><span class="sxs-lookup"><span data-stu-id="10f5c-161">views</span></span>

* <span data-ttu-id="10f5c-162">Modèles</span><span class="sxs-lookup"><span data-stu-id="10f5c-162">models</span></span>

* <span data-ttu-id="10f5c-163">Regroupement</span><span class="sxs-lookup"><span data-stu-id="10f5c-163">bundling</span></span>

* <span data-ttu-id="10f5c-164">Filtres</span><span class="sxs-lookup"><span data-stu-id="10f5c-164">filters</span></span>

* <span data-ttu-id="10f5c-165">Ouvrez une session en entrée/sortie, identité (cela dans l’étape suivante du didacticiel.)</span><span class="sxs-lookup"><span data-stu-id="10f5c-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="10f5c-166">Contrôleurs et vues</span><span class="sxs-lookup"><span data-stu-id="10f5c-166">Controllers and views</span></span>

* <span data-ttu-id="10f5c-167">Chacune des méthodes copier à partir de l’ASP.NET MVC `HomeController` au nouveau `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="10f5c-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="10f5c-168">Notez que dans ASP.NET MVC, type de retour de méthode de modèle prédéfini contrôleur action est [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); dans ASP.NET MVC de base, les méthodes d’action retournés `IActionResult` à la place.</span><span class="sxs-lookup"><span data-stu-id="10f5c-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="10f5c-169">`ActionResult` implémente `IActionResult`, il est donc inutile de modifier le type de retour de vos méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="10f5c-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="10f5c-170">Copiez les fichiers de vues Razor *About.cshtml*, *Contact.cshtml* et *Index.cshtml* du projet ASP.NET MVC dans le projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="10f5c-171">Exécutez l’application ASP.NET Core et chaque méthode de test.</span><span class="sxs-lookup"><span data-stu-id="10f5c-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="10f5c-172">Nous n’avons pas encore effectué la migration l’ou les styles de mise en page, par conséquent, les vues de rendu contient uniquement le contenu dans les fichiers de vue.</span><span class="sxs-lookup"><span data-stu-id="10f5c-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="10f5c-173">Comme vous ne disposez pas des liens générés du fichier de mise en page pour les vues `About` et `Contact`, vous devez les appeler à partir du navigateur (remplacez **4492** par le numéro de port utilisé dans votre projet).</span><span class="sxs-lookup"><span data-stu-id="10f5c-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Page de contact](mvc/_static/contact-page.png)

<span data-ttu-id="10f5c-175">Notez l’absence de styles et d’éléments de menu.</span><span class="sxs-lookup"><span data-stu-id="10f5c-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="10f5c-176">Nous le corrigerons dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="10f5c-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="10f5c-177">Contenu statique</span><span class="sxs-lookup"><span data-stu-id="10f5c-177">Static content</span></span>

<span data-ttu-id="10f5c-178">Dans les versions précédentes d’ASP.NET MVC, contenu statique a été hébergé à partir de la racine du projet web et a été inséré avec fichiers côté serveur.</span><span class="sxs-lookup"><span data-stu-id="10f5c-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="10f5c-179">Dans ASP.NET Core, le contenu statique est hébergé dans le dossier *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="10f5c-180">Vous devez copier le contenu statique de votre ancienne application ASP.NET MVC dans le dossier *wwwroot* de votre projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="10f5c-181">Dans cette exemple de conversion :</span><span class="sxs-lookup"><span data-stu-id="10f5c-181">In this sample conversion:</span></span>

* <span data-ttu-id="10f5c-182">Copie le *favicon.ico* fichier à partir de l’ancien projet MVC dans le *wwwroot* dossier dans le projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="10f5c-183">L’ancien ASP.NET MVC project utilise [Bootstrap](https://getbootstrap.com/) pour que son style et le programme d’amorçage des fichiers dans le *contenu* et *Scripts* dossiers.</span><span class="sxs-lookup"><span data-stu-id="10f5c-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="10f5c-184">Le modèle, ce qui a généré l’ancien projet ASP.NET MVC, fait référence à l’amorçage dans le fichier de disposition (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="10f5c-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="10f5c-185">Vous pouvez copier le *bootstrap.js* et *bootstrap.css* à partir de l’ASP.NET MVC, les fichiers de projet pour le *wwwroot* dossier dans le nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="10f5c-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="10f5c-186">Au lieu de cela, nous allons ajouter la prise en charge pour le démarrage (et autres bibliothèques côté client) à l’aide du CDN dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="10f5c-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="10f5c-187">Migrer le fichier de disposition</span><span class="sxs-lookup"><span data-stu-id="10f5c-187">Migrate the layout file</span></span>

* <span data-ttu-id="10f5c-188">Copiez le fichier *_ViewStart.cshtml* à partir du dossier *Views* de l’ancien projet ASP.NET MVC dans le dossier *Views* du projet de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="10f5c-189">Le fichier *_ViewStart.cshtml* n’a pas changé dans ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="10f5c-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="10f5c-190">Créez un dossier *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="10f5c-191">*Facultatif :* copie *_ViewImports.cshtml* à partir de la *FullAspNetCore* du projet MVC *vues* dossier dans le projet de ASP.NET Core  *Vues* dossier.</span><span class="sxs-lookup"><span data-stu-id="10f5c-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="10f5c-192">Supprimez toute déclaration d’espace de noms dans le *_ViewImports.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="10f5c-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="10f5c-193">Le *_ViewImports.cshtml* fournit des espaces de noms pour tous les fichiers de l’affichage de fichiers et permet de bénéficier de [programmes d’assistance de balise](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="10f5c-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="10f5c-194">Programmes d’assistance de balise sont utilisés dans le nouveau fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="10f5c-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="10f5c-195">Le *_ViewImports.cshtml* fichier est une nouveauté pour ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="10f5c-196">Copiez le fichier *_Layout.cshtml* à partir du dossier *Views/Shared* de l’ancien projet ASP.NET MVC dans le dossier *Views/Shared* du projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="10f5c-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="10f5c-197">Ouvrez *_Layout.cshtml* et apportez les modifications suivantes (le code complet est indiqué ci-dessous) :</span><span class="sxs-lookup"><span data-stu-id="10f5c-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="10f5c-198">Remplacez `@Styles.Render("~/Content/css")` par un élément `<link>` pour charger *bootstrap.css* (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="10f5c-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="10f5c-199">Supprimez `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="10f5c-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="10f5c-200">Commentez la ligne `@Html.Partial("_LoginPartial")` (en la plaçant entre `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="10f5c-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="10f5c-201">Nous y reviendrons dans un prochain didacticiel.</span><span class="sxs-lookup"><span data-stu-id="10f5c-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="10f5c-202">Remplacez `@Scripts.Render("~/bundles/jquery")` par un élément `<script>` (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="10f5c-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="10f5c-203">Remplacez `@Scripts.Render("~/bundles/bootstrap")` par un élément `<script>` (voir ci-dessous).</span><span class="sxs-lookup"><span data-stu-id="10f5c-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="10f5c-204">Le balisage de remplacement pour l’inclusion d’amorçage CSS :</span><span class="sxs-lookup"><span data-stu-id="10f5c-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="10f5c-205">Le balisage de remplacement de jQuery et d’inclusion de programme d’amorçage JavaScript :</span><span class="sxs-lookup"><span data-stu-id="10f5c-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="10f5c-206">La mise à jour *_Layout.cshtml* fichier est présenté ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="10f5c-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="10f5c-207">Afficher le site dans le navigateur.</span><span class="sxs-lookup"><span data-stu-id="10f5c-207">View the site in the browser.</span></span> <span data-ttu-id="10f5c-208">Il doit maintenant charger correctement, avec les styles attendus en place.</span><span class="sxs-lookup"><span data-stu-id="10f5c-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="10f5c-209">*Facultatif :* Vous pouvez essayer d’utiliser le nouveau fichier de mise en page.</span><span class="sxs-lookup"><span data-stu-id="10f5c-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="10f5c-210">Pour ce projet, copiez le fichier de mise en page à partir du projet *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="10f5c-211">Le nouveau fichier de mise en page utilise les [Tags Helpers](xref:mvc/views/tag-helpers/intro) et comprend d’autres améliorations.</span><span class="sxs-lookup"><span data-stu-id="10f5c-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="10f5c-212">Configurer le regroupement et minimisation</span><span class="sxs-lookup"><span data-stu-id="10f5c-212">Configure bundling and minification</span></span>

<span data-ttu-id="10f5c-213">Pour plus d’informations sur la configuration du regroupement et de la minimisation, consultez [Regroupement et Minimisation](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="10f5c-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="10f5c-214">Résoudre les erreurs HTTP 500</span><span class="sxs-lookup"><span data-stu-id="10f5c-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="10f5c-215">De nombreux problèmes peuvent provoquer un message d’erreur HTTP 500 qui ne donne aucune information sur la source du problème.</span><span class="sxs-lookup"><span data-stu-id="10f5c-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="10f5c-216">Par exemple, si le fichier *Views/_ViewImports.cshtml* contient un espace de noms qui n’existe pas dans votre projet, vous obtenez une erreur HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="10f5c-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="10f5c-217">Par défaut dans les applications ASP.NET Core, la `UseDeveloperExceptionPage` extension est ajoutée à la `IApplicationBuilder` et exécutées lorsque la configuration est *développement*.</span><span class="sxs-lookup"><span data-stu-id="10f5c-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="10f5c-218">Cela est détaillée dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="10f5c-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="10f5c-219">ASP.NET Core convertit les exceptions non gérées dans une application web des réponses d’erreur HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="10f5c-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="10f5c-220">Normalement, les détails de l’erreur ne sont pas inclus dans ces réponses pour éviter la divulgation d’informations sensibles sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="10f5c-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="10f5c-221">Consultez **à l’aide de la Page d’Exception Developer** dans [gérer les erreurs](../fundamentals/error-handling.md) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="10f5c-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10f5c-222">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="10f5c-222">Additional resources</span></span>

* [<span data-ttu-id="10f5c-223">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="10f5c-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="10f5c-224">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="10f5c-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
